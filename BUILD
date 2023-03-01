# This files builds a local copy of https://github.com/BlueBrain/HighFive.
# A local copy is currently used as the repository does not use bazel and
# therefore does not have a BUILD file.

# If possible, it should be updated to use bazel's `rules_forein_cc`.
# https://bazelbuild.github.io/rules_foreign_cc/main/index.html

# Currently, when `bazel build highfive` is run from this directory, it simply
# calls cmake and make using a genrule.
# As the WORKSPACE file is at the root of the exai3 repo, output from this
# build will mirror the path to this file from the root.
# This build creates a directory named `header_lib` as it is a header-only
# library, so the directory will be available at:
# /<path to exai3/exai3/bazel-bin/third_party/HighFive/header_lib.

# Just take all files in local directory as sources.
filegroup(
    name = "srcs",
    srcs = glob(["**"]),
    visibility = ["//visibility:public"],
)

# The HighFive repo files are intended to be used as includes.
# A filegroup is created for easier inclusion in other bazel builds, it can be
# accessed with //third_party/HighFive:includes.

# Files listed were determined by the output of
# `find . header_lib | grep hpp`
# which was run after a normal build using cmake and make install.

_output_include_files = [
        "header_lib/include/highfive/H5Easy.hpp",
        "header_lib/include/highfive/H5DataType.hpp",
        "header_lib/include/highfive/H5File.hpp",
        "header_lib/include/highfive/H5Reference.hpp",
        "header_lib/include/highfive/H5Attribute.hpp",
        "header_lib/include/highfive/bits/H5Path_traits.hpp",
        "header_lib/include/highfive/bits/H5Node_traits.hpp",
        "header_lib/include/highfive/bits/H5Path_traits_misc.hpp",
        "header_lib/include/highfive/bits/H5Attribute_misc.hpp",
        "header_lib/include/highfive/bits/H5Annotate_traits_misc.hpp",
        "header_lib/include/highfive/bits/H5Node_traits_misc.hpp",
        "header_lib/include/highfive/bits/H5_definitions.hpp",
        "header_lib/include/highfive/bits/H5Slice_traits.hpp",
        "header_lib/include/highfive/bits/H5Slice_traits_misc.hpp",
        "header_lib/include/highfive/bits/H5DataSet_misc.hpp",
        "header_lib/include/highfive/bits/H5Converter_misc.hpp",
        "header_lib/include/highfive/bits/H5Exception_misc.hpp",
        "header_lib/include/highfive/bits/H5Selection_misc.hpp",
        "header_lib/include/highfive/bits/H5Annotate_traits.hpp",
        "header_lib/include/highfive/bits/H5PropertyList_misc.hpp",
        "header_lib/include/highfive/bits/H5DataType_misc.hpp",
        "header_lib/include/highfive/bits/H5Utils.hpp",
        "header_lib/include/highfive/bits/H5ReadWrite_misc.hpp",
        "header_lib/include/highfive/bits/H5Dataspace_misc.hpp",
        "header_lib/include/highfive/bits/H5File_misc.hpp",
        "header_lib/include/highfive/bits/H5Iterables_misc.hpp",
        "header_lib/include/highfive/bits/H5FileDriver_misc.hpp",
        "header_lib/include/highfive/bits/H5Object_misc.hpp",
        "header_lib/include/highfive/bits/H5Reference_misc.hpp",
        "header_lib/include/highfive/H5Version.hpp",
        "header_lib/include/highfive/H5Object.hpp",
        "header_lib/include/highfive/H5DataSet.hpp",
        "header_lib/include/highfive/H5FileDriver.hpp",
        "header_lib/include/highfive/H5Group.hpp",
        "header_lib/include/highfive/h5easy_bits/H5Easy_misc.hpp",
        "header_lib/include/highfive/h5easy_bits/H5Easy_scalar.hpp",
        "header_lib/include/highfive/h5easy_bits/H5Easy_public.hpp",
        "header_lib/include/highfive/h5easy_bits/H5Easy_xtensor.hpp",
        "header_lib/include/highfive/h5easy_bits/H5Easy_opencv.hpp",
        "header_lib/include/highfive/h5easy_bits/H5Easy_vector.hpp",
        "header_lib/include/highfive/h5easy_bits/H5Easy_Eigen.hpp",
        "header_lib/include/highfive/H5DataSpace.hpp",
        "header_lib/include/highfive/H5PropertyList.hpp",
        "header_lib/include/highfive/H5Exception.hpp",
        "header_lib/include/highfive/H5Utility.hpp",
        "header_lib/include/highfive/H5Selection.hpp",
]

filegroup(
    name = "includes",
    visibility = ["//visibility:public"],
    srcs = _output_include_files,
)

genrule(
    name = "highfive",
    visibility = ["//visibility:public"],
    srcs = [":srcs"],
    outs = _output_include_files,
    cmd = """

# Create the build directory. As WORKSPACE is in the exai3 root dir must
# specify the path from there.

mkdir -p third_party/HighFive/build && cd third_party/HighFive/build

# Install the `header_lib` directory with cmake and make.

cmake .. -DHIGHFIVE_EXAMPLES=OFF -DCMAKE_INSTALL_PREFIX="../header_lib"
make install

# Contents must be copied over to bazel's specified output location,
# otherwise they will remain in bazel's replica of the current working
# directory. -r flag and ..header_lib/* syntax is necessary for a
# correct copy.

cd ../../..
mkdir -p $(@D)/header_lib && cp -r third_party/HighFive/header_lib/* $(@D)/header_lib/
"""
)
