# Scopes

Scopes are attached to access tokens and specify the actions that can be performed on resources.

Scope | Allows requesting entity to...
---------- | -------
connctd.units.read | query units
connctd.units.admin | create and remove units, includes connctd.units.read
connctd.things.read | query things
connctd.things.action | create actions requests
connctd.connector | create and remove things, update thing properties
connctd.policies.read | query policies
connctd.policies.admin | create and remove policies, includes connctd.policies.read
connctd.core | Can't be requested by oauth 2 apps. Automatically assigend to user token retrieved via /api/v1/auth/login. Combines scopes connctd.policies.admin, connctd.units.admin and connctd.things.read
