# ca-app

![App Architecture](./img/architecture.png?raw=true "App Architecture")

## Components
### Metadata
| Volumes     ||
| :---        | :---                                                                                         |
| `v-src-cam` | shared docker volume **/ca-src** between CAsrc and CAmon. Contains **/in** and **/out**      |
| `v-cam-cas` | shared docker volume **/ca-cas** between CAmon and CAsrv. Contains **studyDate** directories |
| `v-cae-cad` | shared docker volume **/ca-cad** between CAetl and CAdb and CAsrv. Contains **/desc**        |

| Networks    ||
| :---        | :---                                          |
| `n-src-cam` | shared docker network between CAsrc and CAmon |
| `n-cam-cas` | shared docker network between CAmon and CAsrv |
| `n-cas-cae` | shared docker network between CAsrv and CAetl |
| `n-cae-cad` | shared docker network between CAetl and CAdb  |
| `n-cas-cau` | shared docker network between CAsrv and CAui  |

| Users   ||||
| :---    | :---       | :---                                       | :---                                                                                             |
| `u-src` | CAsrc user | Have access to `v-src-cam`                 | Can access, change permissions, ownership and move data files from **/in** to **/out**           |
| `u-cam` | CAmon user | Have access to `v-src-cam` and `v-cam-cas` | Can access data files in **v-src-cam/out** and move to **v-cam-cas**                             |
| `u-cas` | CAsrv user | Have access to `v-cam-cas` and `v-cae-cad` | Can access data file descriptions **/desc**                                                      |
| `u-cae` | CAetl user | Have access to `v-cae-cad`                 | Can do actions in CAdb **data-file-etl** database and create data file descriptions in **/desc** |
| `u-cad` | CAdb user  | Have access to `v-cae-cad`                 | Can read CAdb **data-file-etl** database and notify data file etl pre-processing is done         |
