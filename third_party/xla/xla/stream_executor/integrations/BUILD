load("//xla:xla.bzl", "xla_cc_test")
load("//xla/stream_executor:build_defs.bzl", "stream_executor_friends")
load("@local_tsl//tsl:tsl.bzl", "if_google", "set_external_visibility")
load("@local_tsl//tsl:tsl.default.bzl", "filegroup")
load("@local_tsl//tsl/platform:rules_cc.bzl", "cc_library")

package(
    default_visibility = ["//visibility:public"],
    licenses = ["notice"],
)

# StreamExecutor clients that can depend on targets defined in stream_executor package.
package_group(
    name = "friends",
    packages = stream_executor_friends(),
)

#===--------------------------------------------------------------------------------------------===#
# Exporting headers for Tensorflow
#===--------------------------------------------------------------------------------------------===#

# Tensorflow device memory allocators are aliases for StreamExecutor allocators, we export
# headers for Tensorflow to build shared libraries in OSS.

filegroup(
    name = "device_mem_allocator_headers",
    srcs = [
        "device_host_allocator.h",
        "device_mem_allocator.h",
    ],
    visibility = ["//visibility:public"],
)

#===--------------------------------------------------------------------------------------------===#
# StreamExecutor integrations
#===--------------------------------------------------------------------------------------------===#

# Libraries integrating StreamExecutor into framework APIs, e.g. plugging StreamExecutor device
# memory allocator into Tensorflow.

cc_library(
    name = "tf_allocator_adapter",
    srcs = ["tf_allocator_adapter.cc"],
    hdrs = ["tf_allocator_adapter.h"],
    visibility = ["//visibility:public"],
    deps = [
        "//xla/stream_executor",
        "//xla/stream_executor:device_memory",
        "//xla/stream_executor:device_memory_allocator",
        "//xla/stream_executor:platform",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@local_tsl//tsl/framework:allocator",
        "@local_tsl//tsl/platform:errors",
        "@local_tsl//tsl/platform:statusor",
    ],
)

cc_library(
    name = "device_mem_allocator",
    hdrs = [
        "device_host_allocator.h",
        "device_mem_allocator.h",
    ],
    visibility = ["//visibility:public"],
    deps = [
        "//xla/stream_executor",
        "//xla/stream_executor:memory_allocation",
        "@com_google_absl//absl/base:core_headers",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/synchronization",
        "@local_tsl//tsl/framework:allocator",
        "@local_tsl//tsl/framework:device_id",
        "@local_tsl//tsl/platform:logging",
        "@local_tsl//tsl/profiler/lib:traceme",
    ],
)

xla_cc_test(
    name = "tf_allocator_adapter_test",
    srcs = ["tf_allocator_adapter_test.cc"],
    deps = [
        ":tf_allocator_adapter",
        "//xla/service:cpu_plugin",
        "//xla/service:platform_util",
        "//xla/stream_executor",
        "@com_google_absl//absl/container:flat_hash_set",
        "@com_google_absl//absl/container:node_hash_set",
        "@com_google_absl//absl/log:check",
        "@local_tsl//tsl/platform:statusor",
        "@local_tsl//tsl/platform:test",
        "@local_tsl//tsl/platform:test_main",
    ] + if_google([
        "@local_tsl//tsl/framework:allocator",
    ]),
)
