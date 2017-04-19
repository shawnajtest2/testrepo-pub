2. Capture a system profile on the host with something like
    ```
    sudo perf record -F 99 -a -g -- sleep 30
    ```
3. From inside the container (easier to have this running already in another shell) dump the symbols for the Java process with
    ```
    java -cp attach-main.jar:$JAVA_HOME/lib/tools.jar \
    ```
    ```
    net.virtualvoid.perf.AttachOnce PID
    ```
3. You will now have a `perf-PID.map` file inside `/tmp` of the container. Move this file to the host (I used a mounted volume).
