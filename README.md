# Siite WP Bridge

A small WordPress plugin that lets Siite's own tooling read and correct
Elementor page data over the REST API, so a text, image or layout change on a
client site does not need a developer inside the Elementor editor.

This repository exists for one reason: **it is the update source.** The plugin
declares an `Update URI`, WordPress checks `manifest.json` here, and installs
`siite-wp-bridge.zip` through its own updater. That is why the repository is
public — a private one would need a token in the URL, and that token would then
sit in the plugin file on every site it is installed on.

There are no credentials in the plugin. It authenticates with WordPress
Application Passwords, which are created per site and never leave the site they
belong to.

## Installing

wp-admin → Plugins → Add New → Upload Plugin → `siite-wp-bridge.zip` → activate.

Activation registers a role called **Siite Bridge**: the site's own Editor role
minus every `delete_*`, minus `unfiltered_html` and `upload_files`, plus one
custom capability for writing a menu item. Create a user with that role, give it
an Application Password, and point the tooling at it. Nothing here is meant to
run as an administrator.

## What it does

Reads a document's Elementor tree and reports what is text, what is design, and
what is fed from somewhere else (a global widget, a dynamic tag, the kit's
palette) and therefore cannot be corrected in place. Writes are backed up first,
read back afterwards, and recorded — and a structural change has to declare in
advance which element ids it will add or remove, or it is refused.

## Releasing a new version

1. Bump `SIITE_BRIDGE_VERSION` and the plugin header in the source.
2. Rebuild `siite-wp-bridge.zip` (the plugin folder, one PHP file inside).
3. Set the same version in `manifest.json`.
4. Commit both to `main`.

Every site running the plugin picks it up from there.

## Licence

Not open source. Published for the update mechanism, not for reuse.
