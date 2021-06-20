# ansible-ios-bgp
Role and an example of playbook to configure bgp sessions on Cisco IOS devices, with route-map configuration per neighbor support. This is not possible to configure with the default ios_bgp ansible module alone.

The bgp password can be defined in an encrypted variable file with ansible vault.

You can use also the role "ansible-ios-prefix-list-route-map" to configure the route-maps used here.
https://github.com/wilsonlopes00/ansible-ios-prefix-list-route-map

<br>
<br>

# Variables

You can define default variable values in the file vars.yaml.<br>
Custom variables per neighbor can be defined on task level.<br>
You must define the variables:

**local_as**<br>
**remote_as** <br>
**neighbor_ip**<br>
**password**<br>
**route_map_in**<br>
**route_map_out**<br>

<br>

The **password** variable can be defined in the file secrets.yaml, and encrypted with ansible-vault.

<br>

secrets.yaml

<br>

169.254.0.2_passwd: SOMEPASSWORD
<br>
169.254.0.6_passwd: SOMEOTHERPASSWORD

<br>

To encrypt the file:

```sh
$ ansible-vault encrypt secrets.yaml 
New Vault password: 
Confirm New Vault password: 
Encryption successful

$ cat secrets.yaml 
$ANSIBLE_VAULT;1.1;AES256
30313234373239643336363666393134383034613230643737393137396637623632653638336461
3833326637343164623036663830356132353563656635300a356333396636633132336466663932
34646639326630663663643262306332396565633532376136396537363036326161316564313733
3065323336636530660a62623866YYTFF43236363434393564303039313561383330623033396264
36643864333661623135303034323733343539373530653739663334616561643836666362653239
36323931343537623134623234643861313931UIQQQQ313665313936636433383062666365303136
39613630376336376566643739333737343961383864623333366561373139663534666363663032
34313737623735616337
```
<br>



# Running Playbooks:

With secrets.yaml encrypted, add "--ask-vault-pass" to the command line:

```sh
$ ansible-playbook -i hosts playbook.yaml --ask-vault-pass --ask-pass
```


You can configure the bgp sessions with all neighbors at the same time by changing the variables values.
Example:

```sh
  - name: Configure bgp neighbor with ASN {{ remote_as }} - {{ neighbor_ip }}
    vars:
      local_as: 65000
      remote_as: 65004
      neighbor_ip: 169.254.0.2
      password: '{{ vars["169.254.0.2_passwd"] }}'
      route_map_in: other_route_map_in
      route_map_out: other_route_map_out
    import_role:
      name: ios-bgp
      
    - name: Configure bgp neighbor with ASN {{ remote_as }} - {{ neighbor_ip }}
    vars:
      local_as: 65000
      remote_as: 65005
      neighbor_ip: 169.254.0.6
      password: '{{ vars["169.254.0.6_passwd"] }}'
      route_map_in: other_route_map_in
      route_map_out: other_route_map_out
    import_role:
      name: ios-bgp   
      
 ```

