# mizuRoute
mizuRoute 是一个独立的后处理工具，用于处理水文模型输出的径流数据，并生成河网中的河流流量估计值。该工具最初用于大尺度河流汇流计算（例如美国本土范围内的河网），但同样适用于小型源头流域。

# 仓库内容
本仓库包含 mizuRoute 的源代码（Fortran90）和预处理脚本（Python 与 Bash 脚本）。仓库还包括用户手册、用于测试 mizuRoute 的示例数据集，以及 NetCDF 测试代码（用于确认计算机是否正确加载了 NetCDF 库）。

mizuRoute 的 Fortran 90 源代码由两部分组成：1）河网预处理程序；2）汇流程序。存放在 `ntopo` 中的河网预处理程序用于补充从 GIS 获取的河段连接关系等基础信息，以便后续执行河流汇流计算。存放在 `route` 中的汇流程序负责执行坡面汇流和河道汇流计算。

# 快速开始
1. 获取 mizuRoute 软件包。若只想使用该工具，请点击右侧的“Download Zip”按钮下载软件包。

2. Fortran 编译器。我们已经成功使用 Intel Fortran 编译器（ifort）、GNU Fortran 编译器（gfortran，4.8 或更高版本）以及 PGI Fortran 编译器（pgf90）进行编译，其中后两者可以免费使用。由于我们没有使用任何特定于编译器的扩展，mizuRoute 应该可以使用任意 Fortran 编译器进行编译。如果用户没有 Fortran 编译器，可以免费安装 [gfortran](https://gcc.gnu.org/wiki/GFortran)。最简单的方法是使用包管理器，具体使用哪种包管理器取决于计算机环境。

3. NetCDF 库。[NetCDF](http://www.unidata.ucar.edu/software/netcdf/)（即 Network Common Data Format，网络通用数据格式）是一组软件库和自描述、与机器无关的数据格式，用于支持面向数组的科学数据的创建、访问和共享。mizuRoute 的所有输入/输出均使用 NetCDF。用户需要确保：
NetCDF 4.x 版本已安装在类 Linux 计算机上。
已安装 NetCDF Fortran 库（`libnetcdff.*`），而不仅仅是 C 版本。
NetCDF 库使用的编译器与计划用于编译 mizuRoute 的编译器相同。
用户可以使用 NetCDF 测试代码检查 NetCDF 库是否已正确安装。

4. 编译源代码（河网预处理程序和汇流程序）。完成上述准备后，可以按照以下步骤编译 mizuRoute 源代码：进入本地 mizuRoute 目录，然后进入 `build` 子目录。用户需要分别编译河网预处理程序和汇流程序。
 
    1. 编辑 `F_MASTER` 和 `FC`，将其设置为所需的编译器。如果使用其他 Fortran 编译器，或计算机环境有所不同，可能还需要设置 `NCDF_PATH` 并添加一些额外条目。（如果有人愿意贡献一个真正的 configure 脚本，那将非常有帮助。）

    2. 在 `Makefile` 所在目录下执行 `make`。如果一切顺利，程序会在 `bin` 目录中生成可执行文件 `runoff_route.exe`（或 `process_river_topology.exe`）。根据编译器设置的不同，可能会出现一些警告，但不应出现错误。

    3. 留意 `make` 的输出。可能需要设置一些环境变量，尤其是 `LD_LIBRARY_PATH`，以支持动态链接。

    4. 现在可以运行可执行文件了。

如果顺利完成上述步骤，说明 mizuRoute 已正确安装并可以正常运行。接下来，用户需要处理径流数据和网络拓扑数据。请参阅[用户手册](docs/GMD_routing_v1_user_manual_20150831.pdf)，了解如何为具体应用创建 mizuRoute 输入数据。

建议用户先从示例数据开始，以熟悉整个流程。
对于实际应用，获取河网数据（netCDF）可能是最耗时的环节，因为这通常需要经过 GIS 处理，并将 shapefile 转换为 netCDF。所需信息请参阅第 2.1 节。
