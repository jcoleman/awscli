awscli
======

Installs the awscli command-line tool (the new one, supporting all AWS services).

Configuring
===========

You can control when the awscli tool will be installed during the chef run, either
in the compile stage or in the converge stage.

The attribute `[:awscli][:compile_time]` defaults to `false`, meaning the awscli
will be installed during converge time. It can be set to `true` to install at compile time.
Do this if you need the awscli in your recipes.

This cookbook will optionally configure the awscli tool. If your OpsWorks instance has an IAM
Role then the awscli will pick up that role's credentials automatically from the instance
metadata, so you may not need to configure the access keys. You might still want to configure the
default region.

The following attributes are optional and are not specified by default. If present these 
will be used configure awscli for use as the `root` user:

    [:awscli][:aws_access_key_id]     - The AWS Access Key ID to use.
    [:awscli][:aws_secret_access_key] - The AWS Secret Access Key to use.
    
The optional default region can also be set via this attritbue, which is not specified by default:

    [:awscli][:default_region]        - The default AWS region to use.

Using
=====

If you use the awscli in your recipes, you will probably want to make use of the
library provided by this cookbook, InstanceMetadata. It provides a method
`wait_for_instance_IAM_metadata_to_be_available` which you should call before attempting
to use the awscli.

Here's why: When you are running on an OpsWorks instance with an IAM role, the IAM credentials
are not ready until some time after the instance boots. Calling the awscli before the IAM
credentials are ready will fail.

If you manually install AWS credentials, then you won't need to worry about this.
