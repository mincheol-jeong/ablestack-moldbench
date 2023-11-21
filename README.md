# csbench

This is Apache CloudStack Benchmark Tool, also known as "csbench"! Csbench is a tool designed to evaluate the performance and efficiency of Apache CloudStack. 

The tool is designed to be run from a single host, and can be used to benchmark a single CloudStack Zone.

As of now, there are two modes of operation:
1. Setting up environment with multiple domains, accounts, users, networks, VMs, etc.
2. Benchmarking a list of APIs against an existing CloudStack environment

# Building
1. Install go 1.20 or above. Follow instructions [here](https://go.dev/doc/install) to install golang.
2. Clone the repository
3. Build the binary using the below command. This will generate a binary named `csbench` in the current directory.
```bash
go build
```

# Usage

Setup a config file. Check the sample config file [here](./config/config).

```bash
/csbench$ ./csbench -h
Usage: go run csmetrictool.go -dbprofile <DB profile number>
Options:
  -benchmark
        Benchmark list APIs
  -config string
        Path to config file (default "config/config")
  -create
        Create resources
  -dbprofile int
        DB profile number
  -domain
        Create domain
  -format string
        Format of the report (csv, tsv, table). Valid only for create (default "table")
  -limits
        Update limits to -1
  -network
        Create shared network
  -output string
        Path to output file. Valid only for create
  -teardown
        Tear down all subdomains
  -vm
        Deploy VMs
  -volume
        Attach Volumes to VMs
  -workers int
        number of workers to use while creating resources (default 10)
```

## Setting up an environment for benchmarking
This mode of operation is designed to set up a CloudStack environment with multiple domains, accounts, users, networks and VMs as per the configuration file.

To execute this mode, run the following command followed by the type of resources to be created:
```bash
csbench -create -domain -limits -network -vm -volume
```

This will create the resources under the domain specified in the config file. If there are existing domains, network and VMs present under the domain, they will be used as well for creating the resources.

By default, the number of workers for executing the setup operation is 10. This can be changed by passing the -workers flag followed by the number of workers to be used.

By default the results of setting up the environment are printed out to stdout, if you want to save the results to a file, you can pass the `-output` flag followed by the path to the file. And use `-format` flag to specify the format of the report (`csv`, `tsv`, `table`).

## Benchmarking list APIs
By internally executing a series of APIs, this tool meticulously measures the response times for various users, page sizes, and keyword combinations. 
With its comprehensive benchmarking capabilities, csbench provides invaluable insights into the system's overall performance, allowing cloud administrators 
and developers to fine-tune their configurations for optimal efficiency and seamless user experiences.

Currently, it looks like

```bash
/csbench$ ./csbench -benchmark
```
![image](https://github.com/shapeblue/csbench/assets/3348673/db37e176-474e-4b7d-8323-6a9a919414be)

The following are configurations options 

config/config file looks like the below having, CloudStack URL, user profiles for benchmarking and others,

![image](https://github.com/shapeblue/csbench/assets/3348673/bbdfcbd6-c57d-432f-bd63-799ad63d0b2f)

listCommants.txt file contains the list APIs that will be called for benchmarking

![image](https://github.com/shapeblue/csbench/assets/3348673/51402593-f330-4382-8e6e-4cec79a1bc1a)

Reports will be saved as CSV files for each API under report/individual/<management server host>/ report/accumulated/<management server host>
/individual folder contains the reports for each run
/accumulated folder contains the reports accumulated for all the runs

For example listDomains API Report looks like

![image](https://github.com/shapeblue/csbench/assets/3348673/4182b7ac-217a-489f-b7e6-fcb909633de8)
