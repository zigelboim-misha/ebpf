In this repository we will compile, build and run our eBPF kernel side script.

## Golang Project

First we must initiate a go project:

```shell
go mod init ebpf-test
go mod tidy
```

## Generating our Go files from Clang

Using `go generate` to compile the `.c` file into `.o` and `.go` files.

## Running the eBPF script on the Kernel

By executing `go generate && go build && sudo ./ebpf-test` for a couple of seconds and then using `ctrl+c` we wrote some traces into `/sys/kernel/debug/tracing/trace_pipe`.

They can be viewed by `sudo cat /sys/kernel/debug/tracing/trace_pipe | grep "BPF triggered sys_enter_write"`.

## Enabling Tracing

If your Linux distribution (e.g. Ubuntu) does not have the tracing subsystem enabled by default, you may not see any output. Use the following command to enable this feature:

- `sudo mount -t debugfs none /sys/kernel/debug`
- `sudo echo 1 > /sys/kernel/debug/tracing/tracing_on`

## Dockerfile

The Dockerfile contains everything that is needed to compile the `.c` code, build the `.go` files and run it on the kernel:

Build the image using `docker build -t ebpf2go .`
Run the Dockerfile using `docker run --privileged -v C:\Path\To\Files:/ebpf ebpf2go:latest`

## MacOS

In the `dockerfile` there is a need to change the existing `ln` to `ln -s /usr/include/x86_64-linux-gnu/asm /usr/include/asm`.
