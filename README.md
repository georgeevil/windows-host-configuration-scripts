# Configuraton script for Windows

## What does it do

- Downloads the script to the directory the command is being run from

- Determines the latest version of osquery, fluentbit and telegraf

- Downloads configuration files from this git repository

- Installs osquery, fluentbit and telegraf

- Subsitutes values for data center, hostname, customer id, data ingest token and observe endpoint in configuration files

-  Gets confirmation for overwriting pre-existing configs

- Outputs status of services


## Steps to configure

1. Login to machine via RDP

1. Script flags
    * -customer_id = your observe customer id - REQUIRED
    * -ingest_token = your data stream ingest token from ui - REQUIRED
    * -observe_host_name = host endpoint used in config files - OPTIONAL - defaults to <<customer_id>>collect.observeinc.com
    * -config_files_clean = TRUE/FALSE whether to delete directory created for downloading config files - OPTIONAL - defaults to FALSE
    * -ec2metadata = TRUE/FALSE whether to add ec2 filter section to fluent-bit.conf file - OPTIONAL - defaults to FALSE
        ```
        [FILTER]
            Name aws
            Match *
            imds_version v1
            az true
            ec2_instance_id true
            ec2_instance_type true
            account_id true
            hostname true
            vpc_id true
        ```
    * -datacenter = value to use for datacenter in fluent-bit.conf and telegraf.conf files - OPTIONAL - defaults to AWS
    * -appgroup = value to use for appgroup record in fluent-bit.conf file  - OPTIONAL - defaults to null
    * -local = TRUE/FLASE on whether or not use to local config files instead of the default config files - OPTIONAL - default to FALSE
    * -force = TRUE/FALSE on whether or not overwrite agent config files without confirmation (true will overwrite) - OPTION - default to FALSE




1. Run following script
```
[Net.ServicePointManager]::SecurityProtocol = "Tls, Tls11, Tls12, Ssl3"; Invoke-WebRequest -UseBasicParsing "https://raw.githubusercontent.com/observeinc/windows-host-configuration-scripts/main/agents.ps1" -outfile .\agents.ps1; .\agents.ps1  -ingest_token <<ingest_token>> -customer_id <<customer_id>> -observe_host_name <<observe_host_name>>

## Using Local Configs
If you would like to use a custom config instead of the default configs, you can use the `-local` flag and set it to `TRUE`.  If you do this, the script will look in the directory it is being run from for the files `o
