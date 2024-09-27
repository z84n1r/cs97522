java c
COMS 6998 - High Performance Machine Learning
Homework Assignment 1
Fall 2024
Due Date: September 29 2024
Use the Google Cloud platform. (GCP) or your own machine. Make sure that your Google VM or your machine has at last 32GB of RAM to be able to complete the assignments. GCP coupons will be shared with you.
Instructions:
Theoretical questions are identified by Q while coding exercises are identified by C. Submit a tar-archive named with your Columbia UNI (e.g. .tar) that unpacks to
- /dp1.c
- /dp2.c
- /dp3.c
- /dp4.py
- /dp5.py
- /results.pdf
The pdf contains the outputs of the programs and the answers to the questions.
C1                                                                                              20 points
Write a micro-benchmark that investigates the performance of computing the dot-product that takes two arrays of ’float’ (32 bit) as input. The dimension of the vector space and the number of repetitions for the measurement are command line arguments, i.e. a call ’./dp1 1000 10’ performs 10 measurements on a dot product with vectors of size 1000.
Initialize fields in the input vectors to 1.0.
float dp(long N, float *pA, float *pB) {
float R = 0.0;
int j;
for (j=0;jR += pA[j]*pB[j];
return R;
}
Name the program dp1.c and compile with gcc -O3 -Wall -o dp1 dp1.c .
Make sure the code is executed on a platform. that has enough RAM. The 300000000 size runs should not be killed by the system!
Measure the execution time of the function with clock gettime(CLOCK MONOTONIC).
Measure the time for N=1000000 and N=300000000. Perform. 1000 repetitions for the small case and 20 repetitions for the large case. Compute the appropriate mean for the execution time for the second half of the repetitions.
For the average times, compute the bandwidth in GB/sec and throughput in FLOP/sec, and print the result as
N: 1000000 : 9.999999 sec B: 9.999 GB/sec F: 9.999 FLOP/sec
N: 300000000 : 9.999999 sec B: 9.999 GB/sec F: 9.999 FLOP/sec
C2                                                                                         15 points
Perform. the same microbenchmark with
float dpunroll(long N, float *pA, float *pB) {
float R = 0.0;
int j;
for (j=0;jR += pA[j]*pB[j] + pA[j+1]*pB[j+1] \
+ pA[j+2]*pB[j+2] + pA[j+3] * pB[j+3];
return R;
}
C3                                                                                       15 points
Perform. the same microbenchmark with MKL (Intel library), you may need to install a ’module’ to access MKL.
#include 
float bdp(long N, float *pA, float *pB) {
float R = cblas_sdot(N, pA, 1, pB, 1);
return R;
}
C4                                                                                    10 points
Implement the same microbenchmark in python, using numpy arrays as input.
A = np.ones(N,dtype=np.float32)
B = np.ones(N,dtype=np.float32)
# for a simple loop
def dp(N,A,B):
R = 0.0;
for j in range(0,N):
R += A[j]*B[j]
return R
C5                                                                                    10 points
Perform. the same measurements using ’numpy.dot’.
Q1                                                                                   5 points
Explain the rationale and expected consequence of only using the second half of the measurements for the computation of the mean execution time. Moreover, explain what type of mean is appropriate for the calculations, and why.
Q2                                                                                    15 points
Draw a roofline model based on a peak performance of 200 GFLOPS and memory band-width of 30 GB/s. Add a vertical line for the arithmetic intensity. Plot points for the 10 measurements for the average results for each microbenchmark. The roofline model must be ”plotted” using matplotlib or an equivalent package.
Based on your plotted measurements, explain clearly whether the computations are com-pute or memory bound, and why. Discuss the underlying reasons for why these compu-tations differ or don’t across each microbenchmark.
Lastly代 写COMS 6998 High Performance Machine Learning Homework Assignment 1 Fall 2024Python
代做程序编程语言, identify any microbenchmarks that underperform. relative to the roofline, and explain the algorithmic bottlenecks responsible for this performance gap.
Q3                                                                                       5 points
Using the N = 300000000 simple loop as the baseline, explain the the difference in per-formance for the 5 measurements in the C and Python variants. Explain why this occurs by considering the underlying algorithms used.
Q4                                                                                     5 points
Check the result of the dot product computations against the analytically calculated result. Explain your findings, and why the results occur. (Hint: Floating point operations are not exact.)
COMS 6998 - High Performance Machine Learning
Cloud and MKL Setup Instructions
Parijat Dube and Kaoutar El Maghraoui
1 Google Cloud Setup
This setup assumes that you have a functional Google Cloud account and have used the credits provided by the course to create a billing account. You should also have a project linked to the billing account in which you can create VM instances. Refer the follow-ing link on how to create a project with a billing account Creating Managing Projects.
1. Go to the following link: cloud.google.com and click on Console on the top right of the page.
2. Click on Create a VM option.
Make sure that you have the project with the billing account for the course selected.
3. Configure the VM with the required hardware
There are two things that need to be done for the first homework. This is first changing the machine configuration to a machine with higher RAM. In the above screenshot, you can see I have selected a machine with 32 GB RAM.
The second thing that needs to be done is to increase the storage space of the virtual machine from 10GB to at least 30GB. In the above screenshot, you can see I have increased the storage space to 50GB to be on the safer side.
Once you have configured your machine, scroll to the bottom of the page and click on the Create button to create an instance.
4. Go to the dashboard to check out the created VM instance
5. SSH into the created VM instance
2 Intel MKL Library Installation
For this installation, we will be using the Intel OneAPI Basekit. The instructions for installation are given in the following link: Installation using APT manager. There are other options for installation as well, but we suggest following the APT manager installation.
Make sure you have installed wget.
sudo apt install wget
1. Download the key to system key ring
Copy and paste the following command into the SSH terminal of the VM instance:
wget -O-
https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB
| gpg --dearmor | sudo tee /usr/share/keyrings/oneapi-archive-keyring.gpg
> /dev/null
The above command is a single line. Make sure there are no new lines in the command.
2. Add signed entry to apt sources and configure the APT client to use Intel repository
echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg]
https://apt.repos.intel.com/oneapi all main" |
sudo tee /etc/apt/sources.list.d/oneAPI.list
The above command is also a single line. Make sure there are no new lines in the command.
3. Update packages list and repository index
sudo apt update
4. Install MKL basekit
sudo apt install intel-basekit
This is going to take a while to install.
5. Set environment variables
. /opt/intel/oneapi/setvars.sh
Now you are all set to run your code with the MKL library.
3 Running Code with MKL Linkage
This section assumes that you have a functioning code for C3. The commands given below are single line commands and need to be run with any new lines. Make sure to remove new lines before running the commands in the terminal.
Option 1: Using MKL_LINK_TOOL
/opt/intel/oneapi/mkl/2022.2.0/bin/intel64/mkl_link_tool
gcc -O3 -Wall -o dp3 dp3.c
Option 2: Using GCC Flags
gcc -O3 -Wall -o dp3
-I /opt/intel/oneapi/mkl/2022.2.0/include dp3.c
-L /opt/intel/oneapi/mkl/2022.2.0/lib -lmkl_rt





         
加QQ：99515681  WX：codinghelp
