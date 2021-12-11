# LLVM_PGO_PROFILES
collection of PGO Benchmarks for LLMV builds 
These are used with Yuki ユキ clang

## Building Clang with PGO

now have a few *.profraw files copy them in  path/to/stage2/profiles/.
You need to merge these using llvm-profdata (even if you only have one! The profile merge transforms profraw into actual profile data, as well). 
This can be done with /path/to/stage1/llvm-profdata merge -output=/path/to/output/profdata.prof path/to/stage2/profiles/*.profraw.

Now, build your final, PGO-optimized Clang. To do this, you’ll want to pass the following additional arguments to CMake.

-DLLVM_PROFDATA_FILE=/path/to/output/profdata.prof - Use the PGO profile from the previous step.
-DCMAKE_C_COMPILER=/path/to/stage1/clang - Use the Clang we built in step 1.
-DCMAKE_CXX_COMPILER=/path/to/stage1/clang++ - Same as above.
From here, you can build whatever targets you need.

Note:
 You may see warnings about a mismatched profile in the build output. These are generally harmless. To silence them, you can add -DCMAKE_C_FLAGS='-Wno-backend-plugin'
 -DCMAKE_CXX_FLAGS='-Wno-backend-plugin' to your CMake invocation.
 
 
 Some profiles are for 4.14 4.19 and 5.15 kernels
