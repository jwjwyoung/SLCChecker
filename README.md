# Serialization Library Compatibility Checker

## What can SLCChecker do:

SLCChecker analyzes data syntax defined using standard serialization libraries and detect incompatibility across versions, which can lead to upgrade failures. 
It focuses on two widely adopted serialization libraries, [Portocol Buffer](https://developers.google.com/protocol-buffers/docs/proto.) and [Apache Thrift](https://diwakergupta.github.io/thrift-missing-guide/):

  (1). Add/delete required field. 

  (2). The tag number of a field has been changed, the protobuf guidelines suggests each field in the message definition has a unique number. These numbers are used to identify your fields in the message binary format, and should not be changed once your message type is in use.

  (3).  A  required field has been changed to optional. According to the guidelines in protobuf official website, Required Is Forever. You should be very careful about marking fields as required. If at some point you wish to stop writing or sending a required field, it will be problematic to change the field to an optional field - old readers will consider messages without this field to be incomplete and may reject or drop them unintentionally. You should consider writing application-specific custom validation routines for your buffers instead.

For (1) and (2), SLCChecker will output ERROR info, and for (3), SLCChecker will output WARNING info. 


## How to use
1. Checkout SLCChecker to your local machine.

    `git checkout git@github.com:jwjwyoung/SLCChecker.git`

2. Prepare the application that you would like to check the consistentcy on the same machine, suppose its path is `path_app`. 

3. Run Script

    `python3 slcchecker.py  --app path_app --filetype --v1 old_version_tag --v2 new_version_tag`

    e.g. for proto file:

    `python3 slcchecker.py  --app hbase --proto --v1 rel/2.2.6 --v2 rel/2.3.3`
    
    e.g. for thrift file:

    `python3 slcchecker.py  --app hbase --thrift --v1 rel/2.2.6 --v2 rel/2.3.3`
