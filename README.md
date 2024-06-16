
# irTactile Alpha Announcement

After a few weeks of work, I am excited to announce that irTactile has reached alpha status. irTactile is a tool designed to generate audio output to drive bass shakers using iRacing telemetry data.

You might wonder why we need a new tool when SimHub already exists. The answer is simple: I found that the suspension effects in SimHub always felt out of sync with the feedback I experienced through the steering wheel and the audio from the loudspeakers. Others have mentioned similar issues with engine vibrations, often resorting to using regular game audio to drive their shakers.

However, there wasn't an easy solution for suspension effects. I began creating my own profiles in SimHub but quickly ran into limitations. This led me to develop my own application. Unlike SimHub, which attempts to recreate signals using sine waves, irTactile takes a different approach. It directly uses the suspension signals from the telemetry, applies filters, and sends the processed signal to the audio device.

The results so far are promising. The wheel force feedback, regular audio, and bass shakers seem to be in sync. The main challenge now is applying the right filters to ensure a consistent feel across different cars.

In the future releases i will allow creation of custom data processors. In the first release only customs processors are provided, they work well with my steup but not necessary with every other setup. 

The application is in a very early alpha phase. Please do not expect that everything is working perfectly. 

The software is currently closed source, but I will release the sources as soon as i find the time to cleanup the code. 

## Current Features

- Support for multiple audio devices
- Visualization for up to 16 audio channels
- Compatibility with all cars that have regular suspension (Prototypes and Formula cars are currently not supported)
- Pre-configured settings for several cars
- Automatic generation of default configurations for unsupported cars
- Demo mode allowing testing of all channels

I look forward to your feedback and suggestions as irTactile continues to evolve.

# irTactile Documentation

## How To

Running irTactile is straightforward. Follow these steps to get started:

1. **Extract the application**
   - A folder with write access is required, to allow storing configurations
2. **Select the Audio Driver**
   - When you start the application for the first time, you will be prompted to select an audio driver. I recommend choosing "WASAPI" as it offers the lowest latency.
   - To re-run the configuration you have to delete device_config.json in the root directory

3. **Select the Devices**
   - Choose the audio devices you want to use. Note that these devices must support a sampling rate of 48kHz.

4. **Demo Mode**
   - If you run the application in demo mode, a default signal will be sent to the active channel. You can scroll through the channels using the left/right arrow keys.

5. **Regular Mode Configuration**
   - Before running the app in regular mode, you need to assign the individual outputs to the sound card channels. This configuration is done in the `channel_config.json` file. You can use the provided file as a reference.
   
   - For each device and channel where you want to output a signal, the following information is required. 
     ```json
     {
       "deviceID": "DEFAULT_1", // Select the device here
       "channels": [
         {
           "gain": 1, // Master gain for the channel
           "id": 0,  // Channel index. Zero based
           "limiterGain": 10, // Gain reduction factor to be applied when the signal exceeds the threshold
           "limiterGamma": 1.0, // Not used at the moment
           "limiterThreshold": 1.0, // Threshold for the limiter
           "sources": [ // List of sources to be mixed for this channel 
             {
               "gain": 1.0, // Gain for this source
               "streamAttribute": 0, // Individual attribute 
               "streamId": "SUSPENSION" // Data source to use
             }
           ]
         }
       ]
     }
     ```
   - All gain values are simply factors. 1.0 means the signal will not be modified. Values < 1.0 will reduce signal strength. Values > 1.0 will increase the amplitude.
6. **Runtime Configuration**
   - During runtime it is possible to show/hide the audi graph
   - You can enter edit mode to change the size and position of the graph
   - Toggle individual channels

## Available Data Streams

The following data streams can be used in the configuration:

- **ABS**: ABS signal. Cannot be sent directly to a channel.
- **CLUTCH**: Clutch signal. Cannot be sent directly to a channel.
- **ROAD**: Suspension information to emulate road effects.
- **SUSPENSION**: Raw suspension information.
- **SUSPENSION_HF**: Suspension information with a moderate low-pass filter for shakers capable of outputting medium frequencies (e.g., BST300, BST1).
- **SUSPENSION_LFE**: Suspension information with a high low-pass filter for shakers capable of outputting low frequencies (e.g., BST300/LFE/Q10B).
- **SUSPENSION_q10**: Suspension information with a very high low-pass filter for shakers capable of outputting the lowest possible frequencies (e.g., LFE/Q10B).

## Sin Wave Signals

Additionally, there are two sine wave signals defined in `stream_config.json`:
- **ABS_WAVE**
- **ROAD_BASE_WAVE**

If needed, you can define additional signals in `stream_config.json`.

## Example: Driving a Shaker with ABS Signal

To drive a shaker based on the ABS signal, use the ABS signal to control the gain of the channel that is playing a sine wave:

```json
"sources": [
    {
        "gain": 1.0,
        "gainAdaptive": "ABS",
        "streamAttribute": 1,
        "streamId": "ABS_WAVE"
    }
]
```
## Updating per car configuration

For every car you are driving a config file will be created in in the folder "/cars". There are three variables
- maxACC is the most important one, it scales the suspension values in range betwenn 0.0 and 1.0. It has to be selected in a way to cover the maximum range the suspension for the current car delivers. If it is to small, the values will clip. Is it to high range is being lost.
- gamma allows to put more focus on lower or higher acceleration values
- minVel can be used to filter out low velocity movements of the suspension

```json
{
    "gamma": 1.2,
    "maxAcc": 60,
    "minVel": 0.01,
    "name": "Porsche 911 GT3 Cup (992)"
}
```
