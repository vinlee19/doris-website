---
{
    "title": "Elastic scaling",
    "language": "en"
}

---

# Elastic scaling

Doris can easily expand and shrink FE, BE, Broker instances.

## FE Scaling

High availability of FE can be achieved by expanding FE to three top-one nodes.

Users can login to Master FE through MySQL client. By:

`SHOW PROC '/frontends';`

To view the current FE node situation.

You can also view the FE node through the front-end page connection: ``http://fe_hostname:fe_http_port/frontend`` or ```http://fe_hostname:fe_http_port/system?Path=//frontends```.

All of the above methods require Doris's root user rights.

The process of FE node expansion and contraction does not affect the current system operation.

### Adding FE nodes

FE is divided into two roles: Follower and Observer. The Follower role will elect a Follower node as the Master. By default, a cluster can only have one Follower role in the Master state, and there can be multiple Followers and Observers. At the same time, it is necessary to ensure that there are an odd number of Follower roles. All Follower roles form an election group. If the Follower in the Master state goes down, the remaining Followers will automatically elect a new Master to ensure high write availability. Observer synchronizes the data of Master, but does not participate in the election. If only one FE is deployed, the FE is the Master by default.

The first FE to start automatically becomes Master. On this basis, several Followers and Observers can be added.

#### Configure and start Follower or Observer.

 Follower and Observer are configured with Master. The following commands need to be executed at the first startup:

`bin/start_fe.sh --helper host:edit_log_port --daemon`

The host is the node IP of Master, and the edit\_log\_port in Lead's configuration file fe.conf. The --helper is only required when follower/observer is first startup.

#### Add Follower or Observer to the cluster

Add Follower or Observer. Connect to the started FE using mysql-client and execute:

`ALTER SYSTEM ADD FOLLOWER "follower_host:edit_log_port";`

or

`ALTER SYSTEM ADD OBSERVER "observer_host:edit_log_port";`

The follower\_host and observer\_host is the node IP of Follower or Observer, and the edit\_log\_port in its configuration file fe.conf.

View the status of Follower or Observer. Connect to any booted FE using mysql-client and execute:

```SHOW PROC '/frontends';```

You can view the FE currently joined the cluster and its corresponding roles.

> Notes for FE expansion:
>
> 1. The number of Follower FEs (including Masters) must be odd. It is recommended that a maximum of three constituent high availability (HA) modes be deployed.
> 2. When FE is in a highly available deployment (1 Master, 2 Follower), we recommend that the reading service capability of FE be extended by adding Observer FE. Of course, you can continue to add Follower FE, but it's almost unnecessary.
> 3. Usually a FE node can handle 10-20 BE nodes. It is suggested that the total number of FE nodes should be less than 10. Usually three can meet most of the needs.
> 4. The helper cannot point to the FE itself, it must point to one or more existing running Master/Follower FEs.

### Delete FE nodes

Delete the corresponding FE node using the following command:

```ALTER SYSTEM DROP FOLLOWER[OBSERVER] "fe_host:edit_log_port";```

> Notes for FE contraction:
>
> 1. When deleting Follower FE, make sure that the remaining Follower (including Master) nodes are odd.

## BE Scaling

Users can login to Master FE through mysql-client. By:

```SHOW PROC '/backends';```

To see the current BE node situation.

You can also view the BE node through the front-end page connection: ``http://fe_hostname:fe_http_port/backend`` or ``http://fe_hostname:fe_http_port/system?Path=//backends``.

All of the above methods require Doris's root user rights.

The expansion and scaling process of BE nodes does not affect the current system operation and the tasks being performed, and does not affect the performance of the current system. Data balancing is done automatically. Depending on the amount of data available in the cluster, the cluster will be restored to load balancing in a few hours to a day. For cluster load, see the [Tablet Load Balancing Document](../cluster-management/load-balancing).

### Add BE nodes

The BE node is added in the same way as in the **BE deployment** section. The BE node is added by the `ALTER SYSTEM ADD BACKEND` command.

> Notes for BE expansion:
>
> 1. After BE expansion, Doris will automatically balance the data according to the load, without affecting the use during the period.

### Delete BE nodes

There are two ways to delete BE nodes: DROP and DECOMMISSION

The DROP statement is as follows:

```ALTER SYSTEM DROP BACKEND "be_host:be_heartbeat_service_port";```

**Note: DROP BACKEND will delete the BE directly and the data on it will not be recovered!!! So we strongly do not recommend DROP BACKEND to delete BE nodes. When you use this statement, there will be corresponding error-proof operation hints.**

DECOMMISSION clause:

```ALTER SYSTEM DECOMMISSION BACKEND "be_host:be_heartbeat_service_port";```

> DECOMMISSION notes:
>
> 1. This command is used to safely delete BE nodes. After the command is issued, Doris attempts to migrate the data on the BE to other BE nodes, and when all data is migrated, Doris automatically deletes the node.
> 2. The command is an asynchronous operation. After execution, you can see that the BE node's `SystemDecommissioned` status is true through ``SHOW PROC '/backends';` Indicates that the node is offline.
> 3. The order **does not necessarily carry out successfully**. For example, when the remaining BE storage space is insufficient to accommodate the data on the offline BE, or when the number of remaining machines does not meet the minimum number of replicas, the command cannot be completed, and the BE will always be in the state of `SystemDecommissioned` as true.
> 4. The progress of DECOMMISSION can be viewed through `SHOW PROC '/backends';` Tablet Num, and if it is in progress, Tablet Num will continue to decrease.
> 5. The operation can be carried out by:
> 		```CANCEL ALTER SYSTEM DECOMMISSION BACKEND "be_host:be_heartbeat_service_port";```
> 	The order was cancelled. When cancelled, the data on the BE will maintain the current amount of data remaining. Follow-up Doris re-load balancing

**For expansion and scaling of BE nodes in multi-tenant deployment environments, please refer to the [Multi-tenant Design Document](../../admin-manual/resource-admin/multi-tenant).**

## Broker Scaling

There is no rigid requirement for the number of Broker instances. Usually one physical machine is deployed. Broker addition and deletion can be accomplished by following commands:

```ALTER SYSTEM ADD BROKER broker_name "broker_host:broker_ipc_port";```
```ALTER SYSTEM DROP BROKER broker_name "broker_host:broker_ipc_port";```
```ALTER SYSTEM DROP ALL BROKER broker_name;```

Broker is a stateless process that can be started or stopped at will. Of course, when it stops, the job running on it will fail. Just try again.

