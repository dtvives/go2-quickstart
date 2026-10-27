
##### Controller mappings

The table maps controller shortcuts to high-level `SportClient` commands. Availability depends on the model, firmware, and licenses.

| Controller function | Controller shortcut | Go2 SDK command | Notes |
|---|---|---|---|
| Damping Mode / Emergency Stop | `L2 + B` | `Damp()` | Direct match. Stops all joint motion and enters damping mode. |
| Stand Lock / Low Down | `L2 + A` | `StandDown()` / `StandUp()` | Context-dependent toggle between locked standing and low standing. |
| Unlock / Default Gait | `START` | `BalanceStand()` | Unlocks the joints and enters balanced standing mode. |
| Running | `L2 + START` | `TrotRun()` | Selects the running trot mode. |
| Pose | `SELECT` | `Pose(true/false)` | Enables or disables pose mode. |
| Normal | `L1 + START` | `StaticWalk()` | Probable SDK equivalent of the controller's normal walking mode. |
| Classic | `D-pad right + START` | `ClassicWalk(true/false)` | Enables or disables classic walking. |
| Free Walk | `D-pad down + START` | `FreeWalk()` | Selects free-walk mode. |
| Free Avoid | Double-click `A` | `FreeAvoid(true/false)` | Enables or disables free walking with obstacle avoidance. |
| Cross Step | Double-click `B` | `CrossStep(true/false)` | Enables or disables cross-step walking. |
| Bound | Double-click `X` | `FreeBound(true/false)` | The controller guide combines Bound and Jump in one row; `X` is the probable Bound shortcut. |
| Jump | Double-click `Y` | `FreeJump(true/false)` | The controller guide combines Bound and Jump in one row; `Y` is the probable Jump shortcut. |
| Hand Stand | Double-click `R1` | `HandStand(true)` | Enters handstand mode. |
| Erect / leave handstand | Double-click `R2` | `HandStand(false)` | Probable shortcut for leaving handstand mode. |
| Endurance | `L1 + SELECT` | `EconomicGait()` | Selects the energy-saving/endurance gait. |
| Stand up from a fall | `L2 + X` | `RecoveryStand()` | Recovers to a balanced standing position after falling or lying down. |
| Stretch | `R2 + A` | `Stretch()` | Direct match. |
| Shake Hands | `R2 + B` | `Hello()` | `Hello()` performs the paw-waving/handshake movement. |
| Love | `R2 + Y` | `Heart()` | Named `Heart()` in the current SDK. |
| Greet | `L1 + A` | `Scrape()` | Probable match for the SDK's greeting action. |
| Jump Forward | `R1 + A` | `FrontJump()` | Direct match. |
| Sit Down | `R1 + B` | `Sit()` | Enters the sitting position. |
| Rise from sitting | Probably `R1 + B` again | `RiseSit()` | Not listed separately in the controller guide; likely context-dependent. |
| Pounce | `R1 + X` | `FrontPounce()` | Direct match. |
| Dance 1 | `L1 + B` | `Dance1()` | Direct match. |
| Dance 2 | `L1 + X` | `Dance2()` | Direct match. |
| Avoidance OFF/ON | Hold `Y` or `X` for 3 seconds | `SwitchAvoidMode()` | Switches the obstacle-avoidance mode. |
| Searchlight Switch | `L2 + SELECT` | No `SportClient` equivalent | Lighting is not part of the high-level motion-control interface. |
| Vibration/Buzzer Toggle | Press `F3` three times | No `SportClient` equivalent | Controller/audio function rather than a motion command. |

Sources: [Motion Services V2](https://support.unitree.com/home/en/developer/Motion_Services_Interface_V2.0), [legacy Sports Services](https://support.unitree.com/home/en/developer/sports_services), and the [Unitree SDK2 `SportClient` interface](https://github.com/unitreerobotics/unitree_sdk2/blob/main/include/unitree/robot/go2/sport/sport_client.hpp).
