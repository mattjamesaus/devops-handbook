### Troubleshooting
Unable to connect to Aurora Database with JDBC Driver with error "could not load system variables".
> Aurora proxy has a known race condition issue that results in skipping other queries in proxy buffer. connection might be in hang waiting for query response. During authentication, socket has a timeout, that will result throwing the error you describe.

You can add the following onto the JDBC url to workaround the issue `JDBC://auroradatbase.com:3306?usePipelineAuth=false&useBatchMultiSend=false` where the first part of the string is the database deets.
