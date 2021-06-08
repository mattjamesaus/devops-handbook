## Dynamic Blocks Examples
Create one off dynamic object based on variable being set.
```
  dynamic "route" {
    for_each = var.peer_connection_id == "" ? toset([]) : toset([1])
    content {
      cidr_block     = "10.50.0.0/20"
      gateway_id     =  var.peer_connection_id
    }
  }
```
