#!/bin/bash
basename="$(basename "$0")"
(  # Output the Swift version
  set -x
  swift -version
)
(  # Resolve package dependencies before we patch
  set -x
  swift package resolve
)
find ./patches -name '*.patchscript' -exec {} \;  # Run all the patch scripts
host="$(hostname --short).local"
port="8080"
executable="Run"
configuration="release"
(  # Kill the executable if it's already running
  set -x
  killall "$executable"
)
(  # Setup a background loop that will output the content of the main endpoint once available
  while true; do
    curl --silent http://$host:$port && echo && break
    sleep 10
  done
)&
(  # Build executable and then run it with arguments
  set -x
  swift build --configuration "$configuration" &&
    # Only do `swift run` if it builds successfully.
    # Run `swift run` in the background with `&` at the end.
    # Close stdout (`>&-`) and stderr (`2>&-`), otherwise the post-receive hook will never exit.
    (swift run --configuration "$configuration" --skip-build "$executable" --hostname "$host" --port "$port" >&- 2>&- &)
)

