# Provider Plugin Cache

By default, terraform init downloads plugins into a subdirectory of the working directory so that each working directory is self-contained. As a consequence, if you have multiple configurations that use the same provider then a separate copy of its plugin will be downloaded for each configuration.

Given that provider plugins can be quite large (on the order of hundreds of megabytes), this default behavior can be inconvenient for those with slow or metered Internet connections. Therefore Terraform optionally allows the use of a local directory as a shared plugin cache, which then allows each distinct plugin binary to be downloaded only once.

To enable the plugin cache, use the `plugin_cache_dir` setting in the CLI configuration file. For example:

```
plugin_cache_dir = "$HOME/.terraform.d/plugin-cache"
```

This directory must already exist before Terraform will cache plugins; Terraform will not create the directory itself.

Please note that on Windows it is necessary to use forward slash separators (/) rather than the conventional backslash (\) since the configuration file parser considers a backslash to begin an escape sequence.

Setting this in the configuration file is the recommended approach for a persistent setting. Alternatively, the `TF_PLUGIN_CACHE_DIR` environment variable can be used to enable caching or to override an existing cache directory within a particular shell session:

``` console
export TF_PLUGIN_CACHE_DIR="$HOME/.terraform.d/plugin-cache"
```

When a plugin cache directory is enabled, the terraform init command will still use the configured or implied installation methods to obtain metadata about which plugins are available, but once a suitable version has been selected it will first check to see if the chosen plugin is already available in the cache directory. If so, Terraform will use the previously-downloaded copy.

If the selected plugin is not already in the cache, Terraform will download it into the cache first and then copy it from there into the correct location under your current working directory. When possible Terraform will use symbolic links to avoid storing a separate copy of a cached plugin in multiple directories.
