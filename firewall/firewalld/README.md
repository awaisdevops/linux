# Firewalld Guide

## Overview

`irewalld` manages firewall rules dynamically and provides a more flexible way to configure firewall settings compared to traditional `iptables`.

---

## Zones in Firewalld

In `firewalld`, zones are used to define a set of rules that apply to network connections. The default zone is usually `public`.

To view all available zones:

```bash
firewall-cmd --get-zones
```

To list the details of all zones:

```bash
firewall-cmd --list-all-zones
```

### Changing the Default Zone

To set the default zone to `block` (which blocks all incoming and outgoing traffic):

```bash
firewall-cmd --set-default-zone=block
```

To apply the changes, reload `firewalld`:

```bash
firewall-cmd --reload
```

To revert back to the `public` zone as the default:

```bash
firewall-cmd --set-default-zone=public
```

Reload again to reflect the changes:

```bash
firewall-cmd --reload
```

---

## Checking Firewalld Status

To check the status of the `firewalld` service:

```bash
systemctl status firewalld.service
```

To check if `firewalld` is active:

```bash
firewall-cmd --state
```

To list all currently allowed services:

```bash
firewall-cmd --list-services
```

---

## Allowing Services in Firewalld

To allow specific services in `firewalld`, you can use the following commands.

### Allowing Services

To allow HTTPS:

```bash
firewall-cmd --permanent --add-service=https
```

To allow HTTP:

```bash
firewall-cmd --permanent --add-service=http
```

To allow SSH:

```bash
firewall-cmd --permanent --add-service=ssh
```

### Opening a Specific Port

To open a custom port (e.g., port 1234 for TCP):

```bash
sudo firewall-cmd --permanent --add-port=1234/tcp
```

After making changes, remember to reload the firewall to apply them:

```bash
firewall-cmd --reload
```

To confirm the changes, list the allowed services:

```bash
firewall-cmd --list-services
```

---

## Handling Invalid Services

If you cannot find a service in the list, `firewalld` maintains a database of available services. You can search for services by name.

### Example 1: Adding a Service (e.g., Postfix)

If you attempt to add a service (e.g., `postfix`) but get an error:

```bash
firewall-cmd --permanent --add-service=postfix
# Output: 'Error: INVALID_SERVICE: 'postfix' not among existing services'
```

To find the correct service name, use:

```bash
firewall-cmd --get-services
```

To search for SMTP services, use:

```bash
firewall-cmd --get-services | grep -i smtp
```

Then, add the correct service (e.g., `smtp`):

```bash
firewall-cmd --permanent --add-service=smtp
firewall-cmd --reload
```

List the enabled services to confirm:

```bash
firewall-cmd --list-services
```

### Example 2: Adding a Service (e.g., Rpcbind)

If you get an error when adding `rpcbind`:

```bash
firewall-cmd --permanent --add-service=rpcbind
# Output: 'Error: INVALID_SERVICE: 'rpcbind' not among existing services'
```

Search for available services related to `rpc`:

```bash
firewall-cmd --get-services | grep -i rpc
```

Then, add the correct service (e.g., `rpc-bind`):

```bash
firewall-cmd --permanent --add-service=rpc-bind
firewall-cmd --reload
```

Confirm the services:

```bash
firewall-cmd --list-services
```

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```

### Notes:
- The `README.md` now provides a structured guide for using `firewalld` in Linux, covering zones, adding services, opening ports, and handling invalid services.
- The license section includes an MIT license, but you can modify it to any license you prefer. Just replace the link to the `LICENSE` file or modify the text if necessary.
