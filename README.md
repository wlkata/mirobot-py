# mirobot

## Description

`mirobot` is a python module that can be used to control the [WLkata Mirobot](http://www.wlkata.com/site/index.html)

The library is no longer maintained. Please visit the following link for the latest arm control library: https://github.com/wlkata/WLKATA-Python-SDK-wlkatapython

![Mirobot](/images/Mirobot_Solo_256.jpg)

This library uses the G code protocol to communicate with the Mirobot over a serial connection. The official **G code instruction set** and **driver download** for the Mirobot can be found at the [WLkata Download Page](http://www.wlkata.com/site/downloads.html)

## Installation

`mirobot` requires Python >= 3.6. Use `pip3` to install it:

```bash
pip3 install mirobot-py
```

Make sure to **not** install the `mirobot` package-- that package is unrelated to this one.

## Example Usage

```python3
from mirobot import Mirobot

with Mirobot(portname='COM3', debug=True) as m:
    m.home_individual()

    m.go_to_zero()
```

And that's it! Now if you want to save keystrokes, here's a even more minimal version:

```python3
from mirobot import Mirobot

with Mirobot() as m:
    m.home_simultaneous()
```

The `Mirobot` class can detect existing open serial ports and "guess" which one to use as the Mirobot. There's no need to specify a portname for most cases!

## Documentation

Many of the functions and structures in this library are documented. The documentation is hosted [here](https://rirze.github.io/mirobot-py/). If anything is unclear in the docs, please open a Github issue.

## Differences from source repository

### Credits

Big thanks to Mattew Wachter for laying down the framework for this library-- please check out his links below:

[Matthew Wachter](https://www.matthewwachter.com)

[VT Pro Design](https://www.vtprodesign.com)

### Reasons to fork (and not merge upstream)

While based of the same code initially, this repository has developed in a different direction with opinionated views on how one should use a robotics library. Specifically, there is the problem of 'output' when operating a gcode-programmed machine like Mirobot.

- [Matthew's library](https://github.com/matthewwachter/py-mirobot) takes the traditional approach to recieving output from the robot as they appear. Basically this replicates the live terminal feedback in a client similar to Wlkata's Studio program. The original code has a thread listening the background for new messages and displays them as they appear.

- This repository intends to take a more programmatic approach to this behavior. Specifically it narrows down the path to responsibility by explicitly pairing each command to its output. In a stream-messages-as-they-come approach to output messaging, it is not clear (or atleast easy) to determine which command failed and how to ensure scripts stop execution at exactly the point of failure (and not after). That is why each instruction in this library has a dedicated output, ensuring success and having its own message output as a return value. This approach is a lot harder to construct and relies on adapting to the idiosyncrasies of gcode and Mirobot programming.

In the end, while developing this approach to error responsibility, I realized that this would probably not suit everyone's needs-- sometimes people just want a live feed of output. That is why I think Matthew's continued work would be great for the community. I don't want this repository and its beliefs to consume another. I also do not see a way to combine both approaches-- they are inherently incompatible at the core level.

It is my belief that people who are looking to do significant scripting and logic-testing routines will benefit greatly from this library. People who are looking to use a CLI-friendly framework should instead use Matthew's [`py-mirobot`](https://github.com/matthewwachter/py-mirobot) library.

## License

License: [MIT](https://github.com/rirze/mirobot-py/blob/master/LICENSE)
