# NMEA Data Parser and Logger README

## Overview

The **NMEA Data Parser and Logger** program is a Python-based application designed to read, parse, and analyze NMEA sentences from live serial data or pre-recorded log files. The program provides functionalities to calculate various metrics (like Circular Error Probable - CEP) and outputs the parsed data into Excel files for further analysis.

### Key Features:
- **Live NMEA Data Reading**: Read and parse live NMEA data from one or more serial ports simultaneously.
- **Log File Parsing**: Parse NMEA sentences from various log file formats (e.g., `.txt`, `.log`, `.nmea`, `.csv`, and `.xlsx`).
- **Comprehensive NMEA Sentence Support**: Handles multiple NMEA sentence types like GGA, RMC, GSV, GSA, VTG, GLL, ZDA, GNS, GST, and others.
- **CEP Calculation**: Calculates Circular Error Probable (CEP) statistics such as CEP50, CEP68, CEP90, CEP95, and CEP99, based on user-defined reference point or the mean point of the GPS data.
- **Excel Export**: Outputs parsed data, summary statistics, and satellite information to Excel for reference and further analysis.
- **Multi-threading Support**: Supports reading from multiple devices in parallel in live data mode.
- **Customizable Reference Point**: Users can provide a custom reference point for CEP calculation or allow the program to calculate the mean point from data.

---

## Table of Contents
1. [Installation](#installation)
2. [Usage](#usage)
   - [Running the Program](#running-the-program)
   - [Live Serial Data Mode](#live-serial-data-mode)
   - [Log File Mode](#log-file-mode)
3. [NMEA Sentences Supported](#nmea-sentences-supported)
4. [CEP Calculation](#cep-calculation)
5. [Output Files](#output-files)
6. [Error Handling](#error-handling)
7. [Logging](#logging)
8. [License](#license)

---

## Installation

### Prerequisites:
1. **Python 3.x** installed on your system.
2. Install required Python packages:
   ```bash
   pip install -r requirements.txt
   ```

### Cloning the repository:

```bash
git clone https://github.com/adedolapoadegboye/NMEA-Parser-GUI.git
cd NMEA-Parser-GUI
```

---

## Usage

### Running the Program

1. **Launch the Program**:  
   Run the main script `nmea_data.py` to start the program:
   ```bash
   python main.py
   ```

2. **Choose Mode**:  
   The program will prompt you to choose between two modes of operation:
   - **Mode 1**: Live Serial Data
   - **Mode 2**: Existing Log File Parsing

---

### Mode 1 - Live Serial Data Mode

In **Mode 1**, the program reads live NMEA data from one or more defined serial ports. You will be asked to enter the following parameters:
1. **Port** (e.g., `COM1`, `COM9`, `/dev/ttyUSB0`): The name of the serial port. Ensure no other program is using this port.
2. **Baudrate** (e.g., 4800, 9600, 115200): The baudrate of the serial port. This must be set for all defined ports.
3. **Timeout** (e.g., 0.1, 0.5, 1.0): Time in seconds to wait for serial data before terminating the program.
4. **Test Duration** (e.g., 10, 30, 60): Test duration in seconds. The program will continuously read and parse NMEA sentences until the specified duration is reached.
5. **Custom Reference Point**: You can either provide a true test point (latitude and longitude) or let the program calculate the mean point of the GPS data. This point will be used as the reference point for CEP calculation.

Once the configuration is set, the program will continuously read and parse NMEA sentences and log the raw and parsed data for the defined Test Duration. An example is shown below:

```console
2024-10-15 15:45:06,268 [INFO] Logging setup complete. Logs are being saved to logs/NMEA_20241015_154506268246/console_output_20241015_154506268246.txt
Choose data input mode: 1 = Live Serial Data, 2 = Log file
1
Do you want to provide a custom reference point for CEP calculations? (y/n): 
y
Enter reference latitude: 
49.0000000
Enter reference longitude: 
123.00000000
```


### Mode 2 - Log File Mode

In **Mode 2**, you can parse previously collected NMEA log files. The program accepts files in the following formats:
- `.txt`, `.log`, `.nmea` (Plain text)
- `.csv` (Comma-separated values)
- `.xlsx` (Excel)

You will also have the option to provide a custom reference point for CEP calculation or let the program calculate it from the log data.

```console
C:\PythonProjects\NMEA-Parser-Logger\.venv\Scripts\python.exe C:\PythonProjects\NMEA-Parser-Logger\main.py 
2024-10-15 15:49:12,338 [INFO] Logging setup complete. Logs are being saved to logs/NMEA_20241015_154912338000/console_output_20241015_154912338000.txt
Choose data input mode: 1 = Live Serial Data, 2 = Log file
2
Enter the path to the log file (.txt, .log, .nmea, .csv, or .xlsx): 
"C:\PythonProjects\NMEA-Parser-Logger\logs\NMEA_20241015_152059723703\nmea_raw_log_COM7_115200_20241015_152059723703.txt"
Do you want to provide a custom reference point for CEP calculations? (y/n): 
y
Enter reference latitude: 
49.00000000
Enter reference longitude: 
123.00000000
```

---

## NMEA Sentences Supported

This program supports parsing the following NMEA sentence types:
- **GGA**: GPS Fix Data
- **RMC**: Recommended Minimum Navigation Information
- **GSV**: Satellites in View
- **GSA**: GNSS DOP and Active Satellites
- **VTG**: Track Made Good and Ground Speed
- **GLL**: Geographic Position
- **ZDA**: Time and Date
- **GNS**: GNSS Fix Data
- **GST**: Pseudorange Error Statistics
- **GRS**: GNSS Range Residuals
- **RLM**: Return Link Message

Each sentence is parsed into its respective fields and stored for further processing. The full parsed sentence is saved in the `logs/{.xlsx}` directory.

---

## CEP Calculation

The program calculates **Circular Error Probable (CEP)** statistics to quantify the positional accuracy of GPS data. CEP is a measure of accuracy and precision, with the following metrics provided:
- **CEP50**: 50% of the points are within this radius from the reference point.
- **CEP68**, **CEP90**, **CEP95**, **CEP99**: The corresponding percentages of points within these radii.

The program supports the following reference points:
- **Mean Point of dataset**: The mean point of the GPS data. A key indicator for precision estimation.
- **True Point**: The true reference point provided by the user. A key indicator for accuracy estimation.



---

## Output Files

The program exports the following data to Excel files in the `logs/` directory:
1. **Parsed NMEA Sentences**: All parsed sentences with associated metadata.
2. **CEP Summary**: CEP metrics and GPS statistics.
3. **Satellite Data**: Information about satellites tracked (e.g., PRN, SNR).

**File Naming Convention**:  
The output files are saved in the format:  
```
logs/NMEA_YYYYMMDD_HHMMSS/nmea_data_mode_X.xlsx
```
Where `X` refers to the mode (1 for live data, 2 for log parsing).

---

## Error Handling

- **Invalid Data**: Proprietary or unrecognized NMEA sentences are ignored, and errors in parsing are logged.
- **Serial Connection Errors**: Issues with the serial port are caught and logged without crashing the program.

---

## Logging

The program logs important actions and errors to both the console and log files. Log files are stored in the `logs/` directory and named in the format:  
```
console_output_YYYYMMDD_HHMMSS.txt
```
All NMEA raw sentences and detailed execution logs are stored for post-processing.

---

## License

This project is licensed under the MIT License.