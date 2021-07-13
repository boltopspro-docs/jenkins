<!-- note marker start -->
**NOTE**: This repo contains only the documentation for the private BoltsOps Pro repo code.
Original file: https://github.com/boltopspro/jenkins/blob/master/README.md
The docs are publish so they are available for interested customers.
For access to the source code, you must be a paying BoltOps Pro subscriber.
If are interested, you can contact us at contact@boltops.com or https://www.boltops.com

<!-- note marker end -->

# jenkins configset

![CodeBuild](https://codebuild.us-west-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiMHhWS3NaREIvbHdVNDlnS0ltTmR6V1pXeGg0QjNNWG5zZU03bDBTYmR3NlZ6ejh1SWtKUzB1RW1ZNUNMQ29NR1M3eDhhOFVERVJxQXloU0RoaGU4NmdVPSIsIml2UGFyYW1ldGVyU3BlYyI6InAxV3gzc1BCNVM2a1BtdG4iLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)

[![BoltOps Badge](https://img.boltops.com/boltops/badges/boltops-badge.png)](https://www.boltops.com)

This configset install [Jenkins](https://jenkins.io/) on AmazonLinux2.

    $ systemctl status jenkins
    ● jenkins.service - LSB: Jenkins Automation Server
       Loaded: loaded (/etc/rc.d/init.d/jenkins; bad; vendor preset: disabled)
       Active: active (running) since Tue 2020-01-14 02:03:32 UTC; 18min ago
         Docs: man:systemd-sysv-generator(8)
      Process: 4765 ExecStart=/etc/rc.d/init.d/jenkins start (code=exited, status=0/SUCCESS)
        Tasks: 38
       Memory: 315.6M
       CGroup: /system.slice/jenkins.service
               └─4812 /etc/alternatives/java -Dcom.sun.akuma.Daemon=daemonized -Djava.awt.headless=true -DJENKINS_HOME=/var/lib/jenkins ...

    Jan 14 02:03:31 ip-172-31-10-238.us-west-2.compute.internal systemd[1]: Starting LSB: Jenkins Automation Server...
    Jan 14 02:03:31 ip-172-31-10-238.us-west-2.compute.internal runuser[4792]: pam_unix(runuser:session): session opened for user jen...=0)
    Jan 14 02:03:32 ip-172-31-10-238.us-west-2.compute.internal runuser[4792]: pam_unix(runuser:session): session closed for user jenkins
    Jan 14 02:03:32 ip-172-31-10-238.us-west-2.compute.internal jenkins[4765]: Starting Jenkins [  OK  ]
    Jan 14 02:03:32 ip-172-31-10-238.us-west-2.compute.internal systemd[1]: Started LSB: Jenkins Automation Server.
    Hint: Some lines were ellipsized, use -l to show in full.
    $

## What are lono configsets?

Lono configsets allow CloudFormation [cfn-init](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-init.html) [configsets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-init.html) that are typically embedded in the template to be reusable.  More info: [Lono Configsets docs](https://lono.cloud/docs/configsets/).

## Usage

Add to project Gemfile:

```ruby
git "jenkins", git: "git@github.com:boltopspro/jenkins"
```

Use `configset` to enable the configset for the blueprint.  Example:

configs/demo/configsets/base.rb:

```ruby
configset("jenkins", resource: "Instance")
```

This adds the configset to the `resource` with the logical id `Instance` in your CloudFormation template.  The configset is added to the [Resources[Instance].Metadata.AWS::CloudFormation::Init](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-init.html) attribute of the `Instance` resource.  You can confirm that it'll be used with the [lono configsets BLUEPRINT](https://lono.cloud/reference/lono-configsets/) command.

    lono configsets demo

## More Examples

Here's an example adding to a `LaunchConfig` resource:

```ruby
configset("jenkins", resource: "LaunchConfig")
```

Here's an example adding to a `LaunchTemplate` resource:

```ruby
configset("jenkins", resource: "LaunchTemplate")
```
