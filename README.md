# ec2-instance-tools

Some simple keystroke-saver tooling for start/stop/ssh of EC2 instances.

* You can list, start, ssh, and stop using only a handy nickname, or "epithet"
* All the pem-file-name details, username, etc are stashed in a handy config file
* The EC2 IP addresses, which change on every EC2 restart, are dynamically looked up for you

# Requirements

* [jq](https://github.com/jqlang/jq) for all processing
* [miller](https://miller.readthedocs.io/en/latest/) for instance-listing
* [github.com/johnkerl/synctool](https://github.com/johnkerl/synctool) for file transfers
* This is a `bash` script

# Example config

Example `~/.eci2.json`:

```
{
  "pems": [
    {
      "region": "us-east-1",
      "pem": "~/.pems/my-us-east-1.pem"
    },
    {
      "region": "us-west-2",
      "pem": "~/.pems/my-us-west-2.pem"
    }
  ],
  "instances": [
    {
      "epithet": "waldo",
      "username": "ubuntu",
      "instance_id": "i-9876fedbba0123456",
      "region": "us-east-1",
      "description": "Here is a description"
    }
  ]
}
```

# On-line help

```
$ ec2i help
Usage: ec2i {command}

Given a config file with mappings from epithets/nicknames to AWS EC2 parameters,
allows you to start, stop, ssh, etc. The config file encapsulates instance IDs
and AWS regions, and manages IP address and PEM paths, etc, so you don't have to
keystroke those out all the time.

Requires:

* jq for all processing
* mlr for instance-listing
* github.com/johnkerl/synctool for file transfers

Sample ~/.ec2i.json:

    {
      "pems": [ { "region": "us-east-1", "pem": "/path/to/my.pem" } ],
      "instances": [
        {
          "epithet": "ephemeron",
          "username": "ubuntu",
          "instance_id": "i-0cafefeedcafef00d",
          "region": "us-east-1",
          "description": "This is used for Project 827"
        }
    }

Commands list:

ec2i list                 Show epithets and parameters
ec2i config               Show the config file with epithet-to-parameter mappings

ec2i start    {epithets}  Start the named instance(s)
ec2i stop     {epithets}  Stop  the named instance(s)
ec2i show     {epithets}  Show config-file parameters for the named instance(s)
ec2i ssh      {epithet}   SSH into the named instance (does not auto-start it)

ec2i sshup    {epithet}   SSH into the named instance with auto-start and wait for ready

ec2i tags     {epithets}  Shows EC2 tags for the named instance(s)
ec2i tag      {epithet} {key} {value} Sets an EC2 tag for the named instance
ec2i untag    {epithet} {key}         Clears an EC2 tag for the named instance

ec2i ip       {epithets}  Shows current IP-address assignment for the named instance(s)
ec2i state    {epithets}  Shows one-word EC2 state for the named instance(s)
ec2i info     {epithets}  Shows full EC2 JSON info for the named instance(s)

ec2i rel-from {epithet} {path}
ec2i rel-to   {epithet} {path}
ec2i rel-del  {epithet} {path}
ec2i pull-pwd {epithet}
ec2i push-pwd {epithet}
```

# Examples

```
$ ec2i list
Epithet Tags              KeyName     Name                InstanceId   InstanceType State
wihti   2.18,py310        kerl        kerl-large-udf-node i-[redacted] t2.large     running
dwergaz 2.15,py310        kerl        kerl-small-udf-node i-[redacted] t2.small     stopped
larch   2.22,py310        kerl        kerl-linux-aarch64  i-[redacted] t4g.xlarge   stopped
anzo    2.23,py310        kerl        kerl-notebookish    i-[redacted] m5.4xlarge   stopped
austrax 2.24,py310        kerl-oregon kerl-us-west-2      i-[redacted] t2.xlarge    stopped

$ ec2i start wihti

$ ec2i ssh wihti

username    ubuntu
instance_id i-[redacted]
region      us-east-1
ip_address  23.22.6.197
pem_file    /path/to/my.pem


ssh -i /path/to/my.pem ubuntu@10.20.30.40
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 6.5.0-1020-aws x86_64)
...

$ ec2i stop wihti
```
