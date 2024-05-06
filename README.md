# ec2-instance-tools

Some simple keystroke-saver tooling for start/stop/ssh of EC2 instances.

This is a WIP. Some how-to information is to come.

# configs

Example `~/.eci2.json`:

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
      "instance_id": "i-9876fedbba0123456",
      "region": "us-east-1",
      "description": "Here is a description"
    }
  ]
}
