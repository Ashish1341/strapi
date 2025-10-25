# Strapi plugin - audit-logs
## Objective
Adds Automated Audit Logging for all content changes performed through Strapi’s Content API.
Captures key metadata (user, content type, timestamps, diff) for every create, update, and delete operation.
Provides a REST endpoint to retrieve, filter, and paginate audit logs.

## Code structure
packages > plugins > audit-logs (here all the code lies)
The plugin only has server code (that is support to create Audit Logs and read them)
The code is developed as a plugin to add on to functionality without impacting the core.

## How does it work

All the code is setup to be discovered and built by the existing build system in this project, using `yarn.lock` file.
Also the `package.json` plays an important role in building.

### Schema
- Each audit-log entry is store in database `audit_logs`.
- Schema file is at location -> audit-logs > server > src > content-types.

### Bootstrap.ts 
- This file allows us to subscribe/hook onto to db lifecycle events.
- On the event we create entries of the audit in `audit_logs`.

### Read audit-logs
- route is defined in the routes > index.ts
- As the plugin name is `audit-logs` we get that as default route path. And then we add others on the same.
- The route points to controller -> which in turn has the code to get one audit record or many

### Access Control
- Config file 
- Bootstrap registers a new permission (read_audit_logs) in Strapi Admin → Roles & Permissions → Plugin Permissions → Audit Logging.
- Now combination of both is used to restrict permission.
