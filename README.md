# EXAFarm
[![Windows](https://github.com/danielkrupinski/GOESP/workflows/Windows/badge.svg)](https://github.com/NotAma666/EXAFarm)

Growtopia autofarming bot specialized in BFG farming that can also multibox (open multiple instances of the game at the same time). 

Currently supports Windows.

## Download
[Release](https://github.com/NotAma666/EXAFarm/releases/tag/2.0.7) 

## How to use

EXAFarm will automatically injects itself into the Growtopia process. You can download the executable (.exe) file and launch it to start using it right away. The executable will launch Growtopia and inject itself.

When Growtopia is open, menu is openable under `INSERT` key.

## Features

EXAFarm can open multiple instances of Growtopia at the same time and farm with them. 

## Common problems

### How do I open menu?

Press <kbd>INSERT</kbd> while focused on Growtopia window.

### I can't open multiple Growtopia windows!

1. If it says "Not Responding", then your PC can't handle so many Growtopia windows opening one after another. Try opening them in smaller amounts.
2. If it doesn't allow opening multiple Growtopia windows at all ("Did you unzip everything right?"), then make sure you opened this program as an administrator.
3. If absolutely nothing works, you can open the windows by yourself. This program will detect already opened Growtopia instances when it launches. For that you can use [Process Hacker](https://github.com/processhacker/processhacker) or [Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer).

## Screenshots

![Injection](https://i.imgur.com/xsEQx8T.png)

![Autobreak](https://i.imgur.com/5DAIavy.png)

![Growscan](https://i.imgur.com/woFLzF0.png)

![Block tracer](https://i.imgur.com/eGOue6m.png)

![Infinite zoom](https://i.imgur.com/UZDDKdl.png)


## How multibox works

Growtopia uses a mutant handle called `\Sessions\1\BaseNamedObjects\Growtopia` which prevents us from opening more than one instance of the game at a time. To circumvent that, we first suspend all the instances of the game (since it'll crash if we just attempt to delete the handles), and only then delete the handles and open a new game.

### Details
hen a new instance of Growtopia.exe is launched, we first suspend all other instances of the game using `SuspendThread`.
Then we get all the handles in the system with `NtQuerySystemInformation` and filter out those that are not owned by the latest Growtopia process). We get the names of the remaining handles by duplicating them with `DuplicateHandle` so that we can retrieve information out of them with `NtQueryObject`. 

Then we filter out handles with incorrect names and end up with the handles that we have to delete. 
To do that, we use `DuplicateHandle` with `DUPLICATE_CLOSE_SOURCE` and specify no process to duplicate to.

Once all the handles are deleted, we launch a new Growtopia process, which will create new handles. Now we resume all the other
Growtopia.exe's with `ResumeThread` and we've ended up with one new Growtopia instance open.

## License

> Copyright (c) 2021 Ama

This project is licensed under the [MIT License](https://opensource.org/licenses/mit-license.php) - see the [LICENSE](LICENSE) file for details.
