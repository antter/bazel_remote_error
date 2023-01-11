workspace(name = "verkada_backend")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")


http_archive(
    name = "io_bazel_rules_docker",
    sha256 = "b1e80761a8a8243d03ebca8845e9cc1ba6c82ce7c5179ce2b295cd36f7e394bf",
    urls = ["https://github.com/bazelbuild/rules_docker/releases/download/v0.25.0/rules_docker-v0.25.0.tar.gz"],
)

http_archive(
		name = "io_bazel_rules_go",
        sha256 = "099a9fb96a376ccbbb7d291ed4ecbdfd42f6bc822ab77ae6f1b5cb9e914e94fa",
        urls = [
            "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.35.0/rules_go-v0.35.0.zip",
            "https://github.com/bazelbuild/rules_go/releases/download/v0.35.0/rules_go-v0.35.0.zip",
        ],
)

http_archive(
	name = "aspect_gcc_toolchain",
	sha256 = "3341394b1376fb96a87ac3ca01c582f7f18e7dc5e16e8cf40880a31dd7ac0e1e",
	strip_prefix = "gcc-toolchain-0.4.2",
	urls = ["https://github.com/aspect-build/gcc-toolchain/archive/refs/tags/0.4.2.tar.gz"],
)

load(
    "@io_bazel_rules_docker//repositories:repositories.bzl",
    container_repositories = "repositories",
)
container_repositories()


load("@io_bazel_rules_docker//container:container.bzl", "container_pull")
container_pull(
	name = "postgres",
	digest = "sha256:1629bc36c63077ef0ef8b6ea7ff1d601a5211019f15f6b3fd03084788dfaae55",
	registry = "registry.hub.docker.com",
	repository = "library/postgres",
)


load("@io_bazel_rules_go//go:deps.bzl", "go_download_sdk", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains(version = "1.18.1")

go_download_sdk(
    name = "go_sdk_linux",
    goarch = "amd64",
    goos = "linux",
    version = "1.18.1",
)

load("@bazel_gazelle//:deps.bzl", "go_repository")
load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies")

gazelle_dependencies(go_sdk = "go_sdk")

# go repos
go_repository(
	name = "com_github_google_go_containerregistry",
	urls = ["https://github.com/google/go-containerregistry/archive/v0.5.1.tar.gz"],
	sha256 = "c3e28d8820056e7cc870dbb5f18b4f7f7cbd4e1b14633a6317cef895fdb35203",
	importpath = "github.com/google/go-containerregistry",
	strip_prefix = "go-containerregistry-0.5.1",
	build_directives = [
		# Silence Go module warnings about unused modules.
		"gazelle:exclude pkg/authn/k8schain",
	],
)

go_repository(
	name = "com_github_pkg_errors",
	importpath = "github.com/pkg/errors",
	sum = "h1:FEBLx1zS214owpjy7qsBeixbURkuhQAwrK5UwLGTwt4=",
	version = "v0.9.1",
)

go_repository(
	name = "in_gopkg_yaml_v2",
	importpath = "gopkg.in/yaml.v2",
	sum = "h1:obN1ZagJSUGI0Ek/LBmuj4SNLPfIny3KsKFopxRdj10=",
	version = "v2.2.8",
)

go_repository(
	name = "com_github_kylelemons_godebug",
	importpath = "github.com/kylelemons/godebug",
	sum = "h1:RPNrshWIDI6G2gRW9EHilWtl7Z6Sb1BR0xunSBf0SNc=",
	version = "v1.1.0",
)

go_repository(
	name = "com_github_ghodss_yaml",
	importpath = "github.com/ghodss/yaml",
	sum = "h1:wQHKEahhL6wmXdzwWG11gIVCkOv05bNOh+Rxn0yngAk=",
	version = "v1.0.0",
)

# gcc toolchain
load("@aspect_gcc_toolchain//toolchain:repositories.bzl", "gcc_toolchain_dependencies")

gcc_toolchain_dependencies()

load("@aspect_gcc_toolchain//toolchain:defs.bzl", "ARCHS", "gcc_register_toolchain")

gcc_register_toolchain(
    name = "gcc_toolchain_aarch64",
    target_arch = ARCHS.aarch64,
)

gcc_register_toolchain(
    name = "gcc_toolchain_x86",
    target_arch = ARCHS.x86_64,
)