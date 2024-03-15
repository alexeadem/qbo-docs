# Container Engine (QCE)
## Quick start

QCE is qbo's service that offers compute instances. Traditional approaches that rely on virtual machines for compute introduce performance challenges and unnecessary overhead. 

Enter QBO: the solution that unleashes the true potential of cloud computing. With a single AsyncAPI across clouds and on-prem environments, QBO streamlines the deployment and management of compute instances, all while delivering unparalleled bare metal performance with Docker-in-Docker (DinD) subsecond deployments. This approach not only simplifies but also accelerates the adoption of modern cloud computing.

QBO is tailored to the demands of resource-intensive environments, making it an optimal choice for AI and ML workloads. Its commitment to optimized performance ensures that your cloud infrastructure meets the unique requirements of advanced computing applications.

> In the following demo we'll do a sub-second deployment of a QBO instance, and we will be accesing the instance via SSH. 


#### 1. Get `qbo` API version
```bash
qbo version | jq .version[]?
```
```json
{
  "qbo": "cli-amd64:stage-4.3.0.59ceb7d"
}
{
  "docker": "24.0.5",
  "qbo": "cloud-dev-4.3.0.dd223852",
  "host": "lux.cloud.qbo.io"
}
```

#### 2. Add an instance
```bash
export INSTANCE_NAME=alex
qbo add instance $INSTANCE_NAME | jq
```

#### 3. Get instance SSH certficate
```bash
qbo get instance $INSTANCE_NAME -k | jq -r '.output[]?.sshconfig | select( . != null)' > $HOME/.qbo/$INSTANCE_NAME.crt
```

> Change certificate permissions
```bash
chmod 600 $HOME/.qbo/$INSTANCE_NAME.crt
```

#### 4. Get instance IP address
```bash
qbo get ipvs $INSTANCE_NAME | jq -r '.ipvs[]? | select(.node | contains("insta")) | .vip'
```
#### 5. Access instance using ssh
```bash
ssh -o StrictHostKeyChecking=no -i $HOME/.qbo/$INSTANCE_NAME.crt qbo@192.168.1.201
```
```
Warning: Permanently added '192.168.1.201' (ECDSA) to the list of known hosts.

     .oO °°°°OOO o.
  .o°        OO    °o.
 OO          OO      OO
OO  .oOOOo   OOOOOo.  OO
O  OO    OO  OO    OO  O
O  OO    OO  OO    OO  O
OO   °OOOOO   °OOO°   OO
 OO      OO          OO
  °o     OO         o° 
     °Oo 00     .oO°    qbo.
         °°°°°°   
Unlocking the power of cloud computing for anyone, anywhere.   


[qbo@instance-94ad0889 ~]$ 
```
---------------

## Commands
|Command             | Argument                            | Options  | Paraemeter | Admin | Example                                                         |    Description               | CLOUD | CE |
|--------------------|-------------------------------------|----------|------------|-------|-----------------------------------------------------------------|------------------------------|-|-|
| qbo add instance   |    char[64]                         | -i     |  char[64]    |  N    | qbo add instance `alex` -i `hub.docker.com/kindest/node:v1.27.2` | Add instance                |X| |
|                    |                                     | -d     |  char[128]   |  N    |                                                                 | Domain name                  |X| |
| qbo add user       |    char[64] |          |                                    |  Y    | qbo add user `alex`                                             | Add user                     |X| | 
|                    |                                     | --admin|              |  Y    | qbo add user --admin `alex`                                     | Add admin user               |X| |
| qbo add network    |    char[64] |          |                                    |  Y    | qbo add net `136.25.15.102 136.25.15.103`                       | Add network                  |X| |
| qbo delete instance|    char[64] |          |                                    |  N    | qbo delete instance `alex`                                      | Delete instance              |X| |
|                    |                                     | -A     |              |  N    | qbo delete instance -A                                          | Delete all instances         |X| |
| qbo delete user    |    char[64] |          |                                    |  Y    | qbo del user `alex`                                             | Delete user                  |X| |
| qbo delete network |    char[64] |          |                                    |  Y    | qbo del net `136.25.15.102 136.25.15.103`                       | Delete network               |X| |
| qbo stop instance  |    char[64] |          |                                    |  N    | qbo stop instance `alex`                                        | Stop instance                |X| |      
|                    |                                     | -A     |              |  N    | qbo stop cluser -A                                              | Stop all instances           |X| |
| qbo start instance |    char[64] |          |                                    |  N    | qbo start instance `alex`                                       | Start instance               |X| |
|                    |                                     | -A     |              |  N    | qbo start instance -A                                           | Start all instances          |X| |
| qbo get ipvs       |    char[64] |          |                                    |  N    | qbo get ipvs `alex`                                             | Get instance load balancers  |X| |
|                    |                                     | -A     |              |  N    | qbo get ipvs -A                                                 | Get all load balancers       |X| |
| qbo get images     |    char[64] |          |                                    |  N    | qbo get images                                                  | Get node images              |X|X|
|                    |                                     | -A     |              |  N    | qbo get images -A                                               | Get all images               |X|X|
| qbo get users      |    char[64] |          |                                    |  N    | qbo get user `alex`                                             | Get user                     |X| |
|                    |                                     | -A     |              |  N    | qbo get users -A                                                | Get all users                |X| |
| qbo get instance   |    char[64] |          |                                    |  N    | qbo get instance `alex`                                         | Get instance                 |X| |
|                    |                                     | -A     |              |  N    | qbo get instance -A                                             | Get all instances            |X|X|
|                    |                                     | -k     |              |  N    | qbo get instance -k `alex`                                      | Get instance kubeconfig      |X| |
| qbo get instance   |    char[64] |          |                                    |  N    | qbo get instance `alex`                                         | Get instance                 |X| |
|                    |                                     | -A     |              |  N    | qbo get instance -A                                             | Get all instances            |X|X|
|                    |                                     | -k     |              |  N    | qbo get instance -k `alex`                                      | Get instance kubeconfig      |X| |
| qbo version        |    char[64] |          |                                    |  N    | qbo version                                                     | Get qbo version              |X| |





