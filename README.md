


https://dl.photoprism.app/docker/armv7/docker-compose.yml

```sh

  photoprism:
    ## Use photoprism/photoprism:preview-armv7 for testing preview builds:
    image: photoprism/photoprism:armv7
    depends_on:
      - mariadb
    ## Don't enable automatic restarts until PhotoPrism has been properly configured and tested!
    ## If the service gets stuck in a restart loop, this points to a memory, filesystem, network, or database issue:
    ## https://docs.photoprism.app/getting-started/troubleshooting/#fatal-server-errors
    restart: unless-stopped
    security_opt:
      - seccomp:unconfined
      - apparmor:unconfined
    ports:
      - "2342:2342" # HTTP port (host:container)
    environment:
      PHOTOPRISM_ADMIN_USER: "admin"                 # superadmin username
      PHOTOPRISM_ADMIN_PASSWORD: "Ph0t0pr1sm"        # initial superadmin password (minimum 8 characters)
      PHOTOPRISM_AUTH_MODE: "password"               # authentication mode (public, password)
      PHOTOPRISM_SITE_URL: "http://192.168.68.68:2342/"  # server URL in the format "http(s)://domain.name(:port)/(path)"
      PHOTOPRISM_ORIGINALS_LIMIT: 5000               # file size limit for originals in MB (increase for high-res video)
      PHOTOPRISM_HTTP_COMPRESSION: "none"            # improves transfer speed and bandwidth utilization (none or gzip)
      PHOTOPRISM_WORKERS: 1                          # Limits the number of indexing workers to reduce system load
      PHOTOPRISM_LOG_LEVEL: "info"                   # log level: trace, debug, info, warning, error, fatal, or panic
      PHOTOPRISM_READONLY: "false"                   # do not modify originals directory (reduced functionality)
      PHOTOPRISM_EXPERIMENTAL: "false"               # enables experimental features
      PHOTOPRISM_DISABLE_CHOWN: "false"              # disables updating storage permissions via chmod and chown on startup
      PHOTOPRISM_DISABLE_WEBDAV: "false"             # disables built-in WebDAV server
      PHOTOPRISM_DISABLE_SETTINGS: "false"           # disables Settings in Web UI
      PHOTOPRISM_DISABLE_TENSORFLOW: "false"         # disables all features depending on TensorFlow
      PHOTOPRISM_DISABLE_FACES: "true"               # disables face detection and recognition (requires TensorFlow)
      PHOTOPRISM_DISABLE_CLASSIFICATION: "false"     # disables image classification (requires TensorFlow)
      PHOTOPRISM_DISABLE_RAW: "true"                 # disables indexing and conversion of RAW files
      PHOTOPRISM_RAW_PRESETS: "false"                # enables applying user presets when converting RAW files (reduces performance)
      PHOTOPRISM_JPEG_QUALITY: 85                    # a higher value increases the quality and file size of JPEG images and thumbnails (25-100)
      PHOTOPRISM_DETECT_NSFW: "false"                # automatically flags photos as private that MAY be offensive (requires TensorFlow)
      PHOTOPRISM_UPLOAD_NSFW: "true"                 # allows uploads that MAY be offensive (no effect without TensorFlow)
      # PHOTOPRISM_DATABASE_DRIVER: "sqlite"         # SQLite is an embedded database that doesn't require a server
      PHOTOPRISM_DATABASE_DRIVER: "mysql"            # use MariaDB 10.5+ or MySQL 8+ instead of SQLite for improved performance
      PHOTOPRISM_DATABASE_SERVER: "mariadb:3306"     # MariaDB or MySQL database server (hostname:port)
      PHOTOPRISM_DATABASE_NAME: "photoprism"         # MariaDB or MySQL database schema name
      PHOTOPRISM_DATABASE_USER: "photoprism"         # MariaDB or MySQL database user name
      PHOTOPRISM_DATABASE_PASSWORD: "Ph0t0pr1sm"     # MariaDB or MySQL database user password
      PHOTOPRISM_SITE_CAPTION: "AI-Powered Photos App"
      PHOTOPRISM_SITE_DESCRIPTION: ""                # meta site description
      PHOTOPRISM_SITE_AUTHOR: ""                     # meta site author
      ## Run/install on first startup (options: update, gpu, tensorflow, davfs, clean):
      # PHOTOPRISM_INIT: "update clean"
      ## Run as a non-root user after initialization (supported: 0, 33, 50-99, 500-600, and 900-1200):
      # PHOTOPRISM_UID: 1000
      # PHOTOPRISM_GID: 1000
      # PHOTOPRISM_UMASK: 0000
    ## Share hardware devices with FFmpeg and TensorFlow (optional):
    ## See: https://www.raspberrypi.com/documentation/accessories/camera.html#driver-differences-when-using-libcamera-or-the-legacy-stack
    # devices:
    #  - "/dev/video11:/dev/video11" # Video4Linux Video Encode Device (h264_v4l2m2m)
    working_dir: "/photoprism" # do not change or remove
    ## Storage Folders: "~" is a shortcut for your home directory, "." for the current directory
    volumes:
      # "/host/folder:/photoprism/folder"                # Example
      - "/mnt/storage/photoprism/originals:/photoprism/originals" # Original media files (DO NOT REMOVE)
      # - "/example/family:/photoprism/originals/family" # *Additional* media folders can be mounted like this
      # - "~/Import:/photoprism/import"                  # *Optional* base folder from which files can be imported to originals
      - "/mnt/storage/photoprism/storage:/photoprism/storage" # *Writable* storage folder for cache, database, and sidecar files (DO NOT REMOVE)

  ## Database Server (recommended)
  ## see https://docs.photoprism.app/getting-started/faq/#should-i-use-sqlite-mariadb-or-mysql
  mariadb:
    restart: unless-stopped
    image: linuxserver/mariadb:latest
    security_opt:
      - seccomp:unconfined
      - apparmor:unconfined
    ## Never store database files on an unreliable device such as a USB flash drive, an SD card, or a shared network folder:
    volumes:
      - "/mnt/storage/database/mariadb:/config" # DO NOT REMOVE
    environment:
      MYSQL_ROOT_PASSWORD: Ph0t0pr1sm
      MYSQL_DATABASE: photoprism
      MYSQL_USER: photoprism
      MYSQL_PASSWORD: Ph0t0pr1sm

```
