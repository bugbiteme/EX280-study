# Manage authentication and authorization (Can be done in CRC)

## Tasks
### Configure the cluster to use and HTPasswd identity provider  
- create user accounts for: `admin`, `leader`, `developer` and `qa-engineer`
- Create a secret called `cluster-users-secret` using the htpasswd credentails
- create an identiy provider called `cluster-users` that reads the `cluster-users-secret` secret


### Account permissions
- `admin` should be able to modify the cluster
- `leader` should be able to create projects
- `developer` and `qa-engineer` should not be able to modify the cluster
- No other user should be able to create a project

### Default account cleanup
- Remove the `kubeadmin` account

### Project creation
- Create three projects: `front-end`, `back-end` and `app-db`
- `leader` user will be the admin of the projects
- `qa-engineer` user will have `view` access to the `app-db` project

### Group management
- As `admin` create three user groups: `leaders`, `developers` and `qa`
- Add the `leader` user to the `leaders` group
- Add the `developer` user to the `developers` group
- Add the `qa-engineer` to the`qa` group
- The `leaders` group should have edit access to `back-end` and `app-db`
- The `qa` grou shou;f be able to `view` the `front-end` project but not `edit` it

  
  
  [back to main](./README.md) 