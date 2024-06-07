# Bukkit, Spigot and Paper Dockerized
[![pipeline status](https://gitlab.com/cmunroe/docker-bukkit/badges/master/pipeline.svg)](https://gitlab.com/cmunroe/docker-bukkit/-/commits/master)

https://gitlab.com/cmunroe/docker-bukkit

* https://hub.docker.com/r/cmunroe/bukkit
* https://hub.docker.com/r/cmunroe/spigot
* https://hub.docker.com/r/cmunroe/papermc

A Docker Bukkit, Spigot and PaperMC server built on Alpine.

## Simple Tags

* https://hub.docker.com/r/cmunroe/bukkit/tags
* https://hub.docker.com/r/cmunroe/spigot/tags
* https://hub.docker.com/r/cmunroe/papermc/tags

We rock two different version styles (latest / version number).

example: 
 
 Bukkit:
 - cmunroe/bukkit:latest
 - cmunroe/bukkit:1.16.2
 
 Spigot:
 - cmunroe/spigot:latest
 - cmunroe/spigot:1.16.2

 PaperMC:
 - cmunroe/papermc:latest
 - cmunroe/papermc:1.16.2


## Running the Server

To start the server and accept the EULA in one fell swoop, just pass the `EULA=true` environment variable to Docker when running the container. I recommend mounting a directory from your host onto `./data` in the container to make map and server data persistent.

`docker run -it -v ./data:/data -p 25565:25565  -e EULA=true --name mc_server cmunroe/bukkit` 
`docker run -it -v ./data:/data -p 25565:25565  -e EULA=true --name mc_server cmunroe/spigot` 
`docker run -it -v ./data:/data -p 25565:25565  -e EULA=true --name mc_server cmunroe/papermc` 

To run in the background (recommended), add the `-d` flag.

**NOTE**: At this point in time, the `-it` flags are **_strongly recommended_** as without them, the server appears to try to hog 100% of the CPU. We are working on a solution.

## Configuration

You can bring your own existing data + configuration and mount it to the `./data` directory when starting the container by using the `-v` option. You may also pass configuration options as environment variables like so:

`docker run -e DIFFICULTY=2 -e MOTD="A non-standard message" -e SPAWN_ANIMALS=false cmunroe/bukkit`

This container will only attempt generate a `server.properties` file if one does not already exist. If you would like to use the configuration tool, be sure that you are not providing a configuration file or that you also set `FORCE_CONFIG=true` in the environment variables.

### Java Memory

To help control memory usage we have `InitRAM` and `MaxRAM` enviornmenent variables. These variables are set to `1g` and `2g` respectively by default. These variables map to `-Xms` and `-Xmx`. 

If you wish to support more players and plugins, it is highly encourage to increase these variables. You can find more information here: https://minecraft.gamepedia.com/Server/Requirements/Dedicated

### Environment Files

Because of the potentially large number of environment variables that you could pass in, you might want to consider using an [environment variable file](https://docs.docker.com/engine/reference/commandline/run/#set-environment-variables--e---env---env-file). 

Example:
```
# env.list
ALLOW_NETHER=false
level-seed=123456789
EULA=true
```

`docker run -d -it --env-file env.list -v $(pwd)/data:/data -p 25565:25565 cmunroe/bukkit`

#### List of Environment Variables

A full list of `server.properties` settings and their corresponding environment variables is included below, along with their defaults

| Configuration Option          | Environment Variable          | Default                                                          |
| ------------------------------|-------------------------------|------------------------------------------------------------------|
| allow-flight                  | ALLOW_FLIGHT                  | `false`                                                          |
| allow-nether                  | ALLOW_NETHER                  | `true`                                                           |
| difficulty                    | DIFFICULTY                    | `1`                                                              |
| enable-command-block          | ENABLE_COMMAND_BLOCK          | `false`                                                          |
| enable-query                  | ENABLE_QUERY                  | `false`                                                          |
| enable-rcon                   | ENABLE_RCON                   | `false`                                                          |
| force-gamemode                | FORCE_GAMEMODE                | `false`                                                          |
| gamemode                      | GAMEMODE                      | `0`                                                              |
| generate-structures           | GENERATE_STRUCTURES           | `true`                                                           |
| generator-settings            | GENERATOR_SETTINGS            |                                                                  |
| hardcore                      | HARDCORE                      | `false`                                                          |
| level-name                    | LEVEL_NAME                    | `world`                                                          |
| level-seed                    | LEVEL_SEED                    |                                                                  |
| level-type                    | LEVEL_TYPE                    | `DEFAULT`                                                        |
| max-build-height              | MAX_BUILD_HEIGHT              |  `256`                                                           |
| max-players                   | MAX_PLAYERS                   | `20`                                                             |
| max-tick-time                 | MAX_TICK_TIME                 | `60000`                                                          |
| max-world-size                | MAX_WORLD_SIZE                | `29999984`                                                       |
| motd                          | MOTD|                         | `"A Minecraft server powered by Docker (image: cmunroe/bukkit)"` |
| network-compression-threshold | NETWORK_COMPRESSION_THRESHOLD | `256`                                                            |
| online-mode                   | ONLINE_MODE                   | `true`                                                           |
| op-permission-level           | OP_PERMISSION_LEVEL           | `4`                                                              |
| player-idle-timeout           | PLAYER_IDLE_TIMEOUT           | `0`                                                              |
| prevent-proxy-connections     | PREVENT_PROXY_CONNECTIONS     | `false`                                                          |
| pvp                           | PVP                           | `true`                                                           |
| resource-pack                 | RESOURCE_PACK                 |                                                                  |
| resource-pack-sha1            | RESOURCE_PACK_SHA1            |                                                                  |
| server-ip                     | SERVER_IP                     |                                                                  |
| server-port                   | SERVER_PORT                   | `25565`                                                          | 
| snooper-enabled               | SNOOPER_ENABLED               | `true`                                                           |
| spawn-animals                 | SPAWN_ANIMALS                 | `true`                                                           |
| spawn-monsters                | SPAWN_MONSTERS                | `true`                                                           |
| spawn-npcs                    | SPAWN_NPCS                    | `true`                                                           |
| view-distance                 | VIEW_DISTANCE                 | `10`                                                             |
| white-list                    | WHITE_LIST                    | `false`                                                          |
| enable-jmx-monitoring         | ENABLE_JMX_MONITORING         | `false`                                                          |
| rcon.port                     | RCON_PORT                     | `25575`                                                          |
| query.port                    | QUERY_PORT                    | `25565`                                                          |
| use-native-transport          | USE_NATIVE_TRANSPORT          | `true`                                                           |
| enable-status                 | ENABLE_STATUS                 | `true`                                                           |
| broadcast-rcon-to-ops         | BROADCAST_RCON_TO_OPS         | `true`                                                           |
| sync-chunk-writes             | SYNC_CHUNK_WRITES             | `true`                                                           |
| op-permission-level           | OP_PERMISSION_LEVEL           | `4`                                                              |
| entity-broadcast-range-percentage | ENTITY_BROADCAST_RANGE_PERCENTAGE | `100`                                                    |
| rcon.password                 | RCON_PASSWORD                 | ` `                                                              |
| rate-limit                    | RATE_LIMIT                    | `0`                                                              |
| broadcast-console-to-ops      | BROADCAST_CONSOLE_TO_OPS      | `true`                                                           |
| function-permission-level     | FUNCTION_PERMISSION_LEVEL     | `2`                                                              |
| enforce-whitelist             | ENFORCE_WHITELIST             | `false`                                                          |
| resource-pack-sha1            | RESOURCE_PACK_SHA1            | ` `                                                              |
| spawn-protection              | SPAWN_PROTECTION              | `16`                                                             |


## Docker-Compose

```
services:
  bukkit:
    stdin_open: true
    restart: always
    tty: true
    env_file:
      - env.list
    volumes:
      - './data:/data'
    ports:
      - '25565:25565'
    image: cmunroe/bukkit
    container_name: minecraft

```


## Credits

This was originally based upon https://github.com/bbriggs/docker-bukkit. 
Modernized by Cameron Munroe to work on gitlab.