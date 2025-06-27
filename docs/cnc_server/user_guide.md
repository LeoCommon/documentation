# C&C Server User Guide

## Schedule A Job For A Ground Station (Sensor)

### create the job

1. On the top menue go to the tab 'Job List'. Now the already scheduled, currendly running and executed jobs are shown in a table.
2. On the right side klick 'Create New'. A pop-up dialog will open.
3. Insert the job name. We currently recommend the naming-pollicy: '`job_type`_`optional_counter`_`optonal_sensorname`_`date`'. Examples: 'iridium_sniffing_25_10_2024', 'get_logs_nr3_sensor_kl_31_02_2025'
4. Select a start date and time, when the job should start. 
	1. Start Date: Klicking on the right end of the input field, opens a calendar where you can select a day.
	2. Start Time: Select a start time. It uses a 24h format.
6. Select a end time and date, when the job should end. This information is important for recording/sniffing jobs. The maximal length of a job is 24h. Some jobs (as 'get_logs') are done in a second, so the end time&date does not play a role for them, however it has to be specified (maybe change this in future).
	1. End Date: Klicking on the right end of the input field, opens a calendar where you can select a day.
	2. End Time: Select a start time. It uses a 24h format.
6. Give a command and some required arguments. A full list of the supported commands is given in the [client repository readme](https://github.com/LeoCommon/client), here are three common examples:
	- Command: `get_full_status`. Arguments: -none- (if -none- the input field must be empty)
	- Command: `get_logs`. Arguments: `service:client.service`
	- Command: `iridium_sniffing`. Arguments: `centerfrequency_mhz:1624;bandwidth_mhz:5`
7. Click the 'Create Fix Job' button. The job will be created with the status 'pending'.
8. Copy the job name. 

### Add Ground Stations (Sensors) To A Job

1. On the top menue go the tab 'Sensor List'.
2. For those sensors, you want to add the job, click on the right side the 'Add Job' button. A pop-up dialog will open.
3. Paste the job name.
4. Klick OK.

Note: Only jobs with the pending status can be added to sensors. As soon as a sensor starts executing the job, the status is set to running. So make sure to add it to all sensors before this happens.


