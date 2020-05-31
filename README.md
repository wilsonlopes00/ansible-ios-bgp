# ansible-ios-bgp
Role and an example of playbook to configure bgp sessions on Cisco IOS devices, with route-map configuration per neighbor support. This is not possible to configure with the default ios_bgp ansible module alone.

You can use also the role "ansible-ios-prefix-list-route-map" to configure the route-maps used here.
https://github.com/wilsonlopes00/ansible-ios-prefix-list-route-map

<br>
<br>

# Variables

You must define the variables:

**remote_as** <br>
**neighbor_ip**<br>
**password**<br>
**route_map_in**<br>
**route_map_out**<br>

<br>

# Running Playbooks:

```sh
$ ansible-playbook -i hosts playbook.yaml --ask-pass
```


You can configure the bgp sessions with any neighbors at the same time by changing the variable values
Example:

```sh
  - name: Configure bgp neighbor with ASN {{ remote_as }} - {{ neighbor_ip }}
    vars:
      remote_as: 65004
      neighbor_ip: 169.254.0.10
      password: SOMESTRONGPASSWORDHERE
      route_map_in: other_route_map_in
      route_map_out: other_route_map_out
    import_role:
      name: ios-bgp
      
    - name: Configure bgp neighbor with ASN {{ remote_as }} - {{ neighbor_ip }}
    vars:
      remote_as: 65005
      neighbor_ip: 10.0.0.2
      password: SOMESTRONGPASSWORDHERE
      route_map_in: other_route_map_in
      route_map_out: other_route_map_out
    import_role:
      name: ios-bgp   
      
 ```

