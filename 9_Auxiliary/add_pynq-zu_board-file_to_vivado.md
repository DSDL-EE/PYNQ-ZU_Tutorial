## Add PYNQ-ZU board file to Vivado

A board file contains metadata of an FPGA board. Vivado needs that metadata when a new project is created.

Download [PYNQ-ZU board file](https://dpoauwgwqsy2x.cloudfront.net/Download/pynq_zu_v1.1.zip), then extract it to a folder (i.e. `D:\xilinx\pynq_zu\board_file`).

## How to board files to Vivado

### Vivado
- For Linux: $HOME/.Xilinx/Vivado/Vivado_init.tcl
- For Windows: %APPDATA%\Roaming\Xilinx\Vivado\Vivado_init.tcl

### Vitis_HLS
- For Linux: $HOME/.Xilinx/HLS_init.tcl
- For Windows: %APPDATA%\Roaming\Xilinx\HLS_init.tcl

### In `Vivado_init.tcl` file, add (example):
- Linux:

```tcl
set_param board.repoPaths [list "/mnt/storage/xilinx/embedded/zynqmp/pynq_zu/board_file"]
```

- Windows: 

```tcl
set_param board.repoPaths [list "D:\xilinx\pynq_zu\board_file"]
```

### Add multiple folders
```tcl
set_param board.repoPaths [list "D:\xilinx\pynq_zu\board_file" "D:\xilinx\pynq_zu\board_file2"]
                                                              ^
                                                            a space  
```