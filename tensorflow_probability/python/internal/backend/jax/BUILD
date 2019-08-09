# Copyright 2019 The TensorFlow Probability Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# ============================================================================
# Description: JAX backend.

licenses(["notice"])  # Apache 2.0

package(
    default_visibility = [
        "//tensorflow_probability:__subpackages__",
    ],
)

exports_files(["LICENSE"])

FILENAMES = [
    "__init__",
    "compat",
    "control_flow",
    "debugging",
    "dtype",
    "errors",
    "functional_ops",
    "initializers",
    "keras",
    "keras_layers",
    "linalg",
    "linear_operator",
    "misc",
    "nn",
    "numpy_array",
    "numpy_logging",
    "numpy_math",
    "numpy_test",
    "ops",
    "random_generators",
    "sets_lib",
    "sparse_lib",
    "test_lib",
    "v1",
    "v2",
    "_utils",
]

[
    genrule(
        name = "rewrite_{}".format(f),
        srcs = ["//tensorflow_probability/python/internal/backend/numpy:{}.py".format(f)],
        outs = ["{}.py".format(f)],
        cmd = "$(location //tensorflow_probability/python/internal/backend/jax:rewrite) $(SRCS) > $@",
        tools = ["//tensorflow_probability/python/internal/backend/jax:rewrite"],
    )
    for f in FILENAMES
]

py_library(
    name = "jax",
    srcs = [":{}.py".format(f) for f in FILENAMES if "_test" not in f],
    deps = [
        # jax dep,
    ],
)

py_library(
    name = "jax_testlib",
    testonly = 1,
    srcs = [":{}.py".format(f) for f in FILENAMES if "_test" in f],
    deps = [
        ":jax",
        # jax dep,
    ],
)

# [internal] load python3.bzl

py_test(
    name = "jax_test",
    srcs = ["numpy_test.py"],
    main = "numpy_test.py",
    shard_count = 5,
    tags = [
        "no-oss-ci",
        "tfp_jax",
    ],
    deps = [
        ":jax",
        ":jax_testlib",
        # absl/testing:parameterized dep,
        # hypothesis dep,
        # tensorflow dep,
        "//tensorflow_probability",
        "//tensorflow_probability/python/internal:test_case",
        "//tensorflow_probability/python/internal:test_util",
    ],
)

py_binary(
    name = "rewrite",
    srcs = ["rewrite.py"],
    python_version = "PY3",
    deps = [
        # absl:app dep,
    ],
)