gcloud auth list
gcloud config list project

sudo apt update
sudo apt install postgresql-13-pglogical

sudo nano /etc/postgresql/13/main/postgresql.conf


wal_level = logical         # minimal, replica, or logical
max_worker_processes = 10   # one per database needed on provider node
                            # one per node needed on subscriber node
max_replication_slots = 10  # one per node needed on provider node
max_wal_senders = 10        # one per node needed on provider node
shared_preload_libraries = 'pglogical'
max_wal_size = 1GB
min_wal_size = 80MB

listen_addresses = '*'         # what IP address(es) to listen on, '*' is all

sudo nano /etc/postgresql/13/main/pg_hba.conf

host    all all 0.0.0.0/0   md5

sudo systemctl restart postgresql@13-main

sudo su - postgres
psql

psql
\c postgres;
CREATE EXTENSION pglogical;
\c orders;
CREATE EXTENSION pglogical;


CREATE USER replication_admin PASSWORD 'DMS_1s_cool!';
ALTER DATABASE orders OWNER TO replication_admin;
ALTER ROLE replication_admin WITH REPLICATION;



\c orders;
SELECT column_name FROM information_schema.columns WHERE table_name = 'inventory_items' AND column_name = 'id';
ALTER TABLE inventory_items ADD PRIMARY KEY (id);
##you can do this at this stage or when I did it in the lab





\c postgres;
GRANT USAGE ON SCHEMA pglogical TO replication_admin;
GRANT ALL ON SCHEMA pglogical TO replication_admin;
GRANT SELECT ON pglogical.tables TO replication_admin;
GRANT SELECT ON pglogical.depend TO replication_admin;
GRANT SELECT ON pglogical.local_node TO replication_admin;
GRANT SELECT ON pglogical.local_sync_status TO replication_admin;
GRANT SELECT ON pglogical.node TO replication_admin;
GRANT SELECT ON pglogical.node_interface TO replication_admin;
GRANT SELECT ON pglogical.queue TO replication_admin;
GRANT SELECT ON pglogical.replication_set TO replication_admin;
GRANT SELECT ON pglogical.replication_set_seq TO replication_admin;
GRANT SELECT ON pglogical.replication_set_table TO replication_admin;
GRANT SELECT ON pglogical.sequence_state TO replication_admin;
GRANT SELECT ON pglogical.subscription TO replication_admin;



\c orders;
GRANT USAGE ON SCHEMA pglogical TO replication_admin;
GRANT ALL ON SCHEMA pglogical TO replication_admin;
GRANT SELECT ON pglogical.tables TO replication_admin;
GRANT SELECT ON pglogical.depend TO replication_admin;
GRANT SELECT ON pglogical.local_node TO replication_admin;
GRANT SELECT ON pglogical.local_sync_status TO replication_admin;
GRANT SELECT ON pglogical.node TO replication_admin;
GRANT SELECT ON pglogical.node_interface TO replication_admin;
GRANT SELECT ON pglogical.queue TO replication_admin;
GRANT SELECT ON pglogical.replication_set TO replication_admin;
GRANT SELECT ON pglogical.replication_set_seq TO replication_admin;
GRANT SELECT ON pglogical.replication_set_table TO replication_admin;
GRANT SELECT ON pglogical.sequence_state TO replication_admin;
GRANT SELECT ON pglogical.subscription TO replication_admin;


GRANT USAGE ON SCHEMA public TO replication_admin;
GRANT ALL ON SCHEMA public TO replication_admin;
GRANT SELECT ON public.distribution_centers TO replication_admin;
GRANT SELECT ON public.inventory_items TO replication_admin;
GRANT SELECT ON public.order_items TO replication_admin;
GRANT SELECT ON public.products TO replication_admin;
GRANT SELECT ON public.users TO replication_admin;


\c orders;
\dt
ALTER TABLE public.distribution_centers OWNER TO replication_admin;
ALTER TABLE public.inventory_items OWNER TO replication_admin;
ALTER TABLE public.order_items OWNER TO replication_admin;
ALTER TABLE public.products OWNER TO replication_admin;
ALTER TABLE public.users OWNER TO replication_admin;
\dt


\q
exit


TASK 3:-

When prompted for a password enter supersecret!

\c orders

When prompted for a password enter supersecret!

GRANT ALL PRIVILEGES ON TABLE [TABLE_NAME] TO "USER_NAME";

\q


TASK 4:-

date --rfc-3339=seconds


When prompted for a password enter supersecret!

\c orders

When prompted for a password enter supersecret!

insert into distribution_centers values(-97.374,25.7617,California FL',11);
\q



gcloud auth login

gcloud projects get-iam-policy <PROJECT_ID>


gcloud sql instances clone INSTANCE_ID  postgres-orders-pitr --point-in-time 'TIMESTAMP'

