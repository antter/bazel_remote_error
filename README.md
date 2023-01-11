This is a minimal repo to show off some strange Bazel behaivor I'm encountering.

First, you should update `.bazelrc` and `BUILD.bazel` to set up however you do remote caching and RBE.

Find bazel's output-base for this repo.

Then you can run `bazel build //...`. You can note that the `container_image` target does not download its layer's `.tar` file, simply by doing `echo {BAZEL_OUTPUT_BASE}/**/*.tar`. This is expected behaivor, since the `--remote_download_minimal` flag is on.

Then you can `bazel test //...`. There are no tests, but its okay, it will still run and give an error at the end that there are no tests. Now you can run `echo {BAZEL_OUTPUT_BASE}/**/*.tar` again and see that the layer `.tar` has been downloaded locally.

This is a small, silly example but these layers can get pretty large in a real world setting. We like to use `bazel test //...` since its a comfortable way to build and test all targets in parallel.