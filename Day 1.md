## Day 1: Linux User Setup with Non-interactive Shell

This scenario outlines the steps to create a new Linux user with a non-interactive shell, suitable for service accounts or users who will primarily access the system via SSH with key-based authentication.

```bash
sudo adduser <username> -s /sbin/nologin

```

The -s flag is used to specify the user's login shell. A shell is the program that provides the command-line interface for the user.

/sbin/nologin is a special type of shell that immediately terminates the login session. If a user with this shell tries to log in interactively (e.g., via SSH to get a command prompt), they will be disconnected, often with a message like "This account is currently not available."

