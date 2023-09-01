
### Bed Mesh Improvements

`BED_MESH_CALIBRATE_FAST`

Wraps the Klipper `BED_MESH_CALIBRATE` command to scale and redistribute the
probe points so that only the appropriate area in `MESH_MIN` and `MESH_MAX` is
probed. This can dramatically reduce probing times for anything that doesn't
fill the first layer of the bed. If the `MESH_MIN` and `MESH_MAX` arguments
are provided to `PRINT_START` it will automatically use this for bed mesh
calibration (so long as a `[bed_mesh]` section is detected in your config).

The following additional configuration options are available from
[globals.cfg](globals.cfg#L5).

* `variable_probe_mesh_padding` - Extra padding around the rectangle defined by
  `MESH_MIN` and `MESH_MAX`.
* `variable_probe_min_count` - Minimum number of probes for partial probing of a
  bed mesh.
* `variable_probe_count_scale` - Scaling factor to increase probe count for
   partial bed probes.

> **Note:** See the [optional section](#bed-mesh) for additional macros.

> **Note:** The bed mesh optimizations are silently disabled for delta printers.

> **Note:** If the `bed_mesh` config includes a [`relative_reference_index`
  ](https://www.klipper3d.org/Bed_Mesh.html#the-relative-reference-index) then
  the index point selected in the optimized mesh will be the point closest to
  the index point in the mesh from the config. However, if a
  `RELATIVE_REFERENCE_INDEX` parameter is supplied directly to this macro it
  will be used unmodified.

`BED_MESH_CHECK`

* `ABORT` - Set to a non-zero value to abort macro processing on an error.
* `MESH_MIN` - See Klipper documentation for `BED_MESH_CALIBRATE`.
* `MESH_MAX` - See Klipper documentation for `BED_MESH_CALIBRATE`.

Checks the `[bed_mesh]` config and optionally supplied parameters. Will warn
(or optionally abort) if `mesh_min` or `mesh_max` could allow a move out of
range during `BED_MESH_CALIBRATE`. This is run implictily at Klipper startup
and as part of `BED_MESH_CALIBRATE_FAST`.

### Bed Surface

Provides a set of macros for selecting different bed surfaces with
corresponding Z offset adjustments to compensate for their thickness. All
available surfaces must be listed in the `variable_bed_surfaces` array.
Corresponding LCD menus for sheet selection and babystepping will be added to
*Setup* and *Tune* if [`lcd_menus.cfg`](#lcd-menus) is included. Any Z offset
adjustments made in the LCD menus, console, or other clients (e.g. Mainsail,
Fluidd) will be applied to the current sheet and persisted across restarts.

#### `ADJUST_SURFACE_OFFSETS`

Adjusts surface offsets to account for changes in the Z endstop position or
probe Z offset. A message to invoke this command will be shown in the console
when a relevant change is made to `printer.cfg`.

* `IGNORE` - Set to 1 to reset the saved offsets without adjusting the surfaces.

#### `LOAD_SURFACE_MESH`

Attempts to load a mesh associated with the specified surface.

* `SURFACE` *(default: current surface)* - Bed surface.

#### `MAKE_SURFACE_MESH`

Generates a mesh associated with the specified surface. If
`variable_start_try_saved_surface_mesh` is true then `START_PRINT` will load
this mesh when the surface is selected (and skip the mesh leveling step if it
was specified).

* `BED` - *(default: 70)* Bed temperature when probing the bed.
* `EXTRUDER` - *(default: `variable_start_extruder_probing_temp`)* Extruder
  temperature when probing the bed.
* `SURFACE` *(default: current surface)* - Bed surface.
* `MESH_MULTIPLIER` *(default: 2)* - Increases the mesh density by the specified
  integer value while preserving the existing mesh points and relative reference
  index. A value of `1` leaves the mesh unmodified, `2` doubles the density, `3`
  triples it, etc. (I.e. if `bed_mesh` specifies `probe_count: 5,5` and
  `MESH_MULTIPLIER=2` then this macro will generate a 9x9 grid, whereas
  `MESH_MULTIPLIER=3` will generate a 13x13 grid.)
* *See Klipper `BED_MESH_CALIBRATE` documentation for additional arguments.*

#### `SET_SURFACE_ACTIVE`

Sets the provided surface active (from one listed in listed in
`variable_bed_surfaces`) and adjusts the current Z offset to match the
offset for the surface. If no `SURFACE` argument is provided the available
surfaces are listed, with active surface preceded by a `*`.

* `SURFACE` - Bed surface with an associated offset.

#### `SET_SURFACE_OFFSET`

Directly sets the the Z offset of `SURFACE` to the value of `OFFSET`. If no
argument for `SURFACE` is provided the current active surface is used. If no
argument for `OFFSET` is provided the current offset is displayed.

* `OFFSET` - New Z offset for the given surface.
* `SURFACE` *(default: current surface)* - Bed surface.

> **Note:** The `SET_GCODE_OFFSET` macro is overridden to update the
> offset for the active surface. This makes the bed surface work with Z offset
> adjustments made via any interface or client.

### Beep

Implements the M300 command (if a corresponding `[output_pin beeper]` section is
present). This command causes the speaker to emit an audible tone.

#### `M300`

Emits an audible tone.

* `P` *(default: `100`)* - Duration of tone (in milliseconds).
* `S` *(default: `1000`)* - Frequency of tone.

### Draw

Provides convenience methods for extruding along a path and drawing purge lines.

> **Note:** The drawing macros require every `extruder` config(s) to have
> correct `nozzle_diameter` and `filament_diameter` settings.

#### DRAW_LINE_TO

Extrudes a line of filament at the specified height and width from the current
coordinate to the supplied XY coordinate (using the currently active extruder).

* `X` *(default: current X position)* - Absolute X coordinate to draw to.
* `Y` *(default: current Y position)* - Absolute Y coordinate to draw to.
* `HEIGHT` *(default: set via `SET_DRAW_PARAMS`)* - Height (in mm) used to
  calculate extrusion volume.
* `WIDTH` *(default: set via `SET_DRAW_PARAMS`)* - Extrusion width in mm.
* `FEEDRATE` *(default: set via `SET_DRAW_PARAMS`)* - Drawing feedrate in mm/m.

> **Note:** The Z axis position must be set prior to caling this macro. The
> `HEIGHT` parameter is used only to calculate the extrusion volume.

#### SET_DRAW_PARAMS

Sets the default parameters used by DRAW_LINE_TO. This is helpful in reducing
`DRAW_LINE_TO` command line lengths (particluarly important when debugging in
the console).

* `HEIGHT` *(optional; 0.2mm at startup)* - Height (in mm) used to
  calculate extrusion volume.
* `WIDTH` *(optional; nozzle diameter at startup)* - Extrusion width in mm.
* `FEEDRATE` *(optional; 1200mm/m at startup)*  - Drawing feedrate in mm/m.

#### DRAW_PURGE_LINE

Moves to a position at the front edge of the first print layer and purges the
specified length of filament as a line (or rows of lines) in front of the
supplied print area. If no print area is specified the purge lines are drawn at
the front edge of the maximum printable area. If no printable area is set it
defaults to the respective axis limits.

* `PRINT_MIN` *(default: `variable_print_min`)* - Upper boundary of print.
* `PRINT_MAX` *(default: `variable_print_max`)* - Lower boundary of print.
* `HEIGHT` *(default: 62.5% of nozzle diameter)* - Extrusion height in mm.
* `WIDTH` *(default: 125% of nozzle diameter)* - Extrusion width in mm.
* `LENGTH` *(default: `variable_start_purge_length`)* - Length of filament
  to purge. *The default in `variable_start_purge_length` is also the amount
  that is automatically purged at print start.*

> **Note:** You must set `variable_print_min` and `variable_print_max` if the
> X and Y axis limits in your config allow your toolhead to move outside the
> printable area (e.g. for dockable probes or purge buckets).

> **Note:** If your print touches the front edge of the bed it will overlap with
> with the extrusions from `DRAW_PURGE_LINE`.

### Fans

Implements scaling parameters that alter the behavior of the M106 command. Once
set, these parameters apply to any fan speed until they are cleared (by default
this happens at the start and end of the print).

#### `SET_FAN_SCALING`

Sets scaling parameters for the extruder fan.

* `BOOST` *(default: `0`)* - Added to the fan speed.
* `SCALE` *(default: `1.0`)* - The `BOOST` value is added an then the fan
  speed is multiplied by `SCALE`.
* `MAXIMUM` *(default: `255`)* - The fan speed is clamped to no larger
  than `MAXIMUM`.
* `MINIMUM` *(default: `0`)* - The fan speed is clamped to no less
  than `MINIMUM`; if this is a non-zero value the fan can be stopped only via
  the M107 command.
* `SPEED` *(optional)* - This specifies a new speed target, otherwise any new
  adjustments will be applied to the unadjusted value of the last set fan speed.

#### `RESET_FAN_SCALING`

* Clears all existing fan scaling factors.

### Filament

#### `LOAD_FILAMENT` / `UNLOAD_FILAMENT`

Loads or unloads filament to the nozzle.

* `LENGTH` *(default: `variable_load_length`)* - The length of filament to load
  or unload.
* `SPEED` *(default: `variable_load_speed`)* - Speed (in mm/m) to feed the
  filament.
* `MINIMUM` *(default: `variable_load_min_temp`)* - Ensures the extruder is heated
   to at least the specified temperature.

#### Marlin Compatibility

* The `M701` and `M702` commands are implemented with a default filament length
  of `variable_load_length`.

### Heaters

Adds scaling parameters that can alter the behavior of the specified heater.
Once set, these parameters apply to any temperature target on that heater until
the scalaing parameters are cleared. A zero target temperature will turn the
heater off regardless of scaling parameters.

#### `SET_HEATER_SCALING`

Sets scaling parameters for the specified heater. If run without any arguments
any currently scaled heaters and thier scaling parameters will be listed. By
default the scaling is cleared at the start and end of a print.

* `HEATER` - The name of the heater to scale.
* `BOOST` *(default: `0.0`)* - Added to a non-zero target temperature.
* `SCALE` *(default: `1.0`)* - Multiplied with the boosted target
  temperature.
* `MAXIMUM` *(default: `max_temp`)* - The target temperature is clamped
  to no larger than `MAXIMUM`. This value must be between `min_temp` and
  `min_temp`, inclusive.
* `MINIMUM` *(default: `min_temp`)* - A non-zero target temperature is
  clamped to no less than `MINIMUM`. This value must be between `min_temp` and
  `min_temp`, inclusive.
* `TARGET` *(optional)* - This specifies a new target temperature, otherwise any
  new adjustments will be applied to the unadjusted value of the last set target
  temperature.

> **Note:** a zero target temperature will turn the heater off regardless of
> scaling parameters.

#### `RESET_HEATER_SCALING`

Clears current heater scaling.

* `HEATER` *(optional)* - The name of the heater to reset.

> **Note:** if no HEATER argument is specified scaling parameters will be reset
> for all heaters.

#### `SET_HEATER_TEMPERATURE_SCALED`

The scaled version of Klipper's `SET_HEATER_TEMPERATURE`. All arguments are the
same and the function is identical, except that scaling values are applied.

#### `TEMPERATURE_WAIT_SCALED`

The scaled version of Klipper's `TEMPERATURE_WAIT`. All arguments are the
same and the function is identical, except that scaling values are applied.

#### Marlin Compatibility

* The chamber heating commands `M141` and `M191` are implemented if a
  corresponding `[heater_generic chamber]` section is defined in the config.
* The `R` temperature parameter from Marlin is implemented for the `M109` and
  `M190` commands. This parameter will cause a wait until the target temperature
  stabilizes (i.e. the normal Klipper behavior for `S`).
* The `S` parameter for the  `M109` and `M190` commands is altered to behave as
  it does in Marlin. Rather than causing a wait until the temperature
  stabilizes, the wait will complete as soon as the temperature target is
  exceeded.
* The `M109`, `M190`, `M191`, `M104`, `M140`, and `M141` are all overridden to
  implement the heater scaling described above.

> **Note:** Both `SET_HEATER_TEMPERATURE` and `TEMPERATURE_WAIT` are **not**
> overriden and will not scale values. This means that heater scaling
> adjustments made in clients like Mainsail and Fluidd will not be scaled
> (because that seemed like the clearest behavior). The
> [custom LCD menus](#lcd-menus) will also replace the temperature controls
> with non-scaling versions. If you use the stock menus you'll get scaled
> values.

### Kinematics

#### `G28`

Extends the `G28` command to add lazy homing by not re-homing already homed axes
when the `O` argument is included (equivalent to the same argument in Marlin).
See Klipper `G28` documentation for general information and detail on the other
arguments.

* `O` - Omits already homed axes from the homing procedure.

> **Note:** If you have a `[homing_override]` section you will need to update
> any `G28` commands in the gcode part to use `G28.6245197` instead (which is
> the renamed version of Klipper's built-in `G28`). Failure to do this will
> cause `G28` commands to error out with the message ***Macro G28 called
> recursively***.

#### `LAZY_HOME`

Homes the specified axes; by default omits any axes that are already homed.

* `AXES` *(default: XYZ)* - List of axes to home.
* `LAZY` *(default: 1)* - Omits already homed axes from the homing procedure.

### Layer Triggers

Provides the capability to run user-specified g-code commands at arbitrary layer
changes.

#### `GCODE_AT_LAYER`

Runs abritrary, user-provided g-code commands at the user-specified layer or
height. If no arguments are specified it will display all currently scheduled
g-code commands along with their associated layer or height.

* `HEIGHT` - Z height (in mm) to run the command. Exactly one of `HEIGHT` or
  `LAYER` must be specified.
* `LAYER` - Layer number (zero indexed) to run the command. Exactly one of
  `HEIGHT` or `LAYER` must be specified. The special value `next` may be
  specified run the command at the next layer change.
* `COMMAND` - The command to run at layer change. Take care to properly quote
  spaces and escape any special characters.
* `CANCEL` *(default: `0`)* - Cancel the commands previously scheduled at the
  given layer or height. If no `COMMAND` argument is provided all commands at
  the specified `LAYER` or `HEIGHT` are cancelled.

#### `CANCEL_ALL_LAYER_GCODE`

Cancels all g-code commands previously scheduled at any layer or height.

#### Convenient Layer Change Macros

* `PAUSE_NEXT_LAYER ...`
  * Schedules the current print to pause at the next layer change. See
    [`PAUSE`](#pause) macro for additional arguments.
* `PAUSE_AT_LAYER  { HEIGHT=<pos> | LAYER=<layer> } ...`
  * Schedules the current print to pause at the specified layer change.
    See [`PAUSE`](#pause) for additional arguments.
* `SPEED_AT_LAYER { HEIGHT=<pos> | LAYER=<layer> } SPEED=<percentage>`
  * Schedules a feedrate adjustment at the specified layer change. (`SPEED`
    parameter behaves the same as the `M220` `S` parameter.)
* `FLOW_AT_LAYER { HEIGHT=<pos> | LAYER=<layer> } FLOW=<percentage>`
  * Schedules a flow-rate adjustment at the specified layer change. (`FLOW`
    parameter behaves the same as the `M221` `S` parameter.)
* `FAN_AT_LAYER { HEIGHT=<pos> | LAYER=<layer> } ...`
  * Schedules a fan adjustment at the specified layer change. See 
    [`SET_FAN_SCALING`](#set_fan_scaling) for additional arguments.
* `HEATER_AT_LAYER { HEIGHT=<pos> | LAYER=<layer> } ...`
  * Schedules a heater adjustment at the specified layer change. See 
    [`SET_HEATER_SCALING`](#set_heater_scaling) for additional arguments.

> **Note:** If any triggers cause an exception the current print will
> abort. The convenience macros above validate their arguments as much as is
> possible to reduce the chances of an aborted print, but they cannot entirely
> eliminate the risk of a macro doing something that aborts the print.

### Park

Implements toolhead parking.

#### `PARK`

Parks the toolhead.

* `P` *(default: `2`)* - Parking mode
  * `P=0` - If current Z-pos is lower than Z-park then the nozzle will be raised
    to reach Z-park height
  * `P=1` - No matter the current Z-pos, the nozzle will be raised/lowered to
    reach Z-park height.
  * `P=2` - The nozzle height will be raised by Z-park amount but never going
    over the machine’s Z height limit.
* `X` *(default: `variable_park_x`)* - Absolute X parking coordinate.
* `Y` *(default: `variable_park_y`)* - Absolute Y parking coordinate.
* `Z` *(default: `variable_park_z`)* - Z parking coordinate applied according
  to the `P` parameter.
* `LAZY` *(default: 1)* - Will home any unhomed axes if needed and will not
  move any axis if already homed and parked (even if `P=2`).

> **Note:** If a print is in progress the larger of the tallest printed layer or
> the current Z position will be used as the current Z position, to avoid
> collisions with already printed objects during a sequential print.

#### Marlin Compatibility

* The `G27` command is implemented with a default `P0` argument.

### Pause, Resume, Cancel

#### `PAUSE`

Pauses the current print.

* `X` *(default: `variable_park_x`)* - Absolute X parking coordinate.
* `Y` *(default: `variable_park_y`)* - Absolute Y parking coordinate.
* `Z` *(default: `variable_park_z`)* - Relative Z parking coordinate
* `E` *(default: `5`)* - Retraction length to prevent ooze.
* `B` *(default: `10`)* - Number of beeps to emit (if `M300` is enabled).

#### `RESUME`

* `E` *(default: `5`)* - Retraction length to prevent ooze.

#### `CANCEL_PRINT`

Cancels the print and performs all the same functions as `PRINT_END`.

#### Marlin Compatibility

* The `M24`, `M25`, `M600`, `M601`, and `M602` commands are all implemented by
  wrapping the above commands.


### Print Start and End

#### `PRINT_START`

Sets up the printer prior to starting a print (called from the slicer's print
start g-code). A target `CHAMBER` temperature may be provided if a 
`[heater_generic chamber]` section is present in the klipper config.
If `MESH_MIN` and `MESH_MAX` are provided, then `BED_MESH_CALIBRATE` will probe
only the area within the specified rectangle, and will scale the number of
probes to the appropriate density (this can dramatically reduce probe times for
smaller prints).

* `BED` - Bed heater starting temperature.
* `EXTRUDER` - Extruder heater starting temperature.
* `CHAMBER` *(optional)* - Chamber heater starting temperature.
* `MESH_MIN` *(optional)* - Minimum x,y coordinate of the first layer.
* `MESH_MAX` *(optional)* - Maximum x,y coordinate of the first layer.
* `NOZZLE_SIZE` *(default: nozzle_diameter)* - Nozzle diameter of the primary
   extruder.
* `LAYERS` *(optional)* - Total number of layers in the print.

These are the customization options you can add to your
`[gcode_macro _km_options]` section to alter `PRINT_START` behavior:

* `variable_start_bed_heat_delay` *(default: 2000)* - This delay (in
  microseconds) is used to allow the bed to stabilize after it reaches it's
  target temperature. This is present to account for the fact that the
  temperature sensors for most beds are located close to the heating element,
  and thus will register as being at the target temperature before the surface
  of the bed is. For larger or thicker beds you may want to increase this value.
  For smaller or thinner beds you may want to disable this entirely by setting
  it to `0`.

* `variable_start_bed_heat_overshoot` *(default: 2.0)* - This value (in degrees
  Celsius) is added to the supplied target bed temperature and use as the
  initial target temperature when preheating the bed. After the bed preheats to
  this target it there is a brief delay before the final target is set. This
  allows the bed to stabilize at it's final temperature more quickly. For
  smaller or thinner beds you may want to reduce this value or disable it
  entirely by setting it to `0.0`.

* `variable_start_end_park_y` *(default: `max`)* - The final Y position of the
  toolhead in the `PRINT_END` macro, to ensure that the toolhead is out of the
  way when the bed is presented for print removal. This can be set to a Y
  coordinate (e.g. `0.0`), `max` to use `stepper_y.position_max`, or `min` to
  use `stepper_y.position_min`.

* `variable_start_extruder_preheat_scale` *(default: 0.5)* - This value is
  multiplied by the target extruder temperature and the result is used as the
  preheat value for the extruder while the bed is heating. This is done to
  reduce oozing from the extruder while the bed is heating or being probed. Set
  to `1.0` to preheat the extruder to the full target temperature, or to `0.0`
  to not preheat the extruder at all until the bed reaches temperature.

* `variable_start_extruder_probing_temp` *(default: 0)* - If set to a non-zero
  value the extruder will be stabilized at the supplied temperature prior to bed
  probing. This is useful for [Voron TAP](
  https://github.com/VoronDesign/Voron-Tap), load cells, and other forms of
  probing that use the nozzle directly. When this value is provided
  `variable_start_extruder_preheat_scale` is ignored.

* `variable_start_level_bed_at_temp` *(default: True if `bed_mesh` configured
  )* - If true the `PRINT_START` macro will run [`BED_MESH_CALIBRATE_FAST`](
  #bed-mesh-improvements) after the bed has stabilized at its target
  temperature.

* `variable_start_home_z_at_temp` *(default: True if `probe:z_virtual_endstop`
  configured)* - Rehomes the Z axis once the bed reaches its target temperature,
  to account for movement during heating.

* `variable_start_clear_adjustments_at_end` *(default: True)* - Clears temporary
  adjustments after the print completes or is cancelled (e.g. feedrate,
  flow percentage).

* `variable_start_purge_clearance` *(default: 5.0)* - Distance (in millimeters)
  between the purge lines and the print area (if a `start_purge_length` is
  provided).

* `variable_start_purge_length` *(default: 0.0)* - Length of filament (in
  millimeters) to purge after the extruder finishes heating and prior to
  starting the print. For most setups `30` is a good starting point.

* `variable_start_purge_prime_length` *(default: 10.0)* - Length of filament (in
  millimeters) to prime the extruder before drawing the purge lines.

* `variable_start_quad_gantry_level_at_temp` *(default: True if
  `quad_gantry_level` configured)* - If true the `PRINT_START` macro will run
  `QUAD_GANTRY_LEVEL` after the bed has stabilized at its target temperature.

* `variable_start_random_placement_max` *(default: 0)* - A positive value
  specifies the +/- distance in the XY axes that the print can be randomly
  relocated (assuming the bed has sufficient space). This can help reduce bed
  wear from repeatedly printing in the same spot. Note that this feature
  requires additional information to determine the proper bounds of the
  relocated print. As such, `START_PRINT` must have valid `MESH_MIN`/`MESH_MAX`
  parameters, and either `MODEL_MIN`/`MODEL_MAX` must be set or the print file
  must include `EXCLUDE_OBJECT_DEFINE` statements with `POLYGON` lists that
  define the bounds of the objects ([see `exclude_object` for more information](
  https://www.klipper3d.org/Exclude_Object.html)).

* `variable_start_random_placement_padding` *(default: 10.0)* - The minimum
  distance the relocated print will be placed from the printable edge of the
  bed.

* `variable_start_try_saved_surface_mesh` *(default: False)* - If enabled and
  `bed_mesh.profiles` contains a matching mesh for the currently select bed
  surface, then the mesh will be loaded from the saved profile (and
  [`BED_MESH_CALIBRATE_FAST`](#bed-mesh-improvements) will be skipped if
  it would have been run otherwise).

* `variable_start_z_tilt_adjust_at_temp`  *(default: True if `z_tilt`
  configured)* - If true the `PRINT_START` macro will run `Z_TILT_ADJUST` after
  the bed has stabilized at its target temperature.

You can further customize the `PRINT_START` macro by declaring your own override
wrapper. This can be useful for things like loading mesh/skew profiles, or any
other setup that may need to be performed prior to printing.

Here's a skeleton of a `PRINT_START` override wrapper:

```
[gcode_macro PRINT_START]
rename_existing: KM_PRINT_START
gcode:

  # Put macro code here to run before PRINT_START.

  KM_PRINT_START {rawparams}

  # Put macro code here to run after PRINT_START but before the print gcode
```

> **Note:** You can use this same pattern to wrap other macros in order to
  account for customizations specific to your printer. E.g. If you have a
  dockable probe you may choose to wrap `BED_MESH_CALIBRATE` with the
  appropriate docking/undocking commands.

#### `PRINT_START` Phases

The recommended slicer start gcode breaks `PRINT_START` into the phases below.
This approach allows for pausing or cancelling, and inserting custom gcode
between the phases (e.g. to set status LEDs, deploy/dock probes, load filament).
The phases are described in order below:

* `_PRINT_START_PHASE_INIT` - Initializes the `PRINT_START` settings, and begins
  heating the bed and chamber.
* `_PRINT_START_PHASE_PREHEAT` - Stabilizes the bed and (if applicable) chamber
  at the target temperatures. Also homes the axes while heating is in progress.
* `_PRINT_START_PHASE_PROBING` - Performs probing operations, including mesh
  bed calibration, quad gantry leveling, etc.
* `_PRINT_START_PHASE_EXTRUDER` - Stabilizes the extruder at the target
  temperature.
* `_PRINT_START_PHASE_PURGE` - Extrudes a purge line in front of the print area.

#### `PRINT_END`

Parks the printhead, shuts down heaters, fans, etc, and performs general state
housekeeping at the end of the print (called from the slicer's print end
g-code).

### Print Status Events

> **Note:** This is a brand new feature that will likely evolve in the near
> future. Statuses will probably be added and/or removed as I get a better sense
> of how they're being used. _**Keep that in mind as you integrate this into
> your own setup.**_

The following events are fired during during the printing process, and the
`GCODE_ON_PRINT_STATUS` command associates custom gcode with these events. This
custom gcode can be used to set LEDs, emit beeps, or perform any other
operations you may want to run at a given status in the printing process.

* `ready` - Printer is ready to receive a job
* `filament_load` - Loading filament
* `filament_unload` - Unloading filament
* `bed_heating` - Waiting for the bed to reach target
* `chamber_heating` - Waiting for the chamber to reach target
* `homing` - Homing any axis
* `leveling_gantry` - Performing quad gantry-leveling
* `calibrating_z` - Performing z-tilt adjustment
* `meshing` - Calibrating a bed mesh
* `extruder_heating` - Waiting for the extruder to reach target
* `purging` - Printing purge line
* `printing` - Actively printing
* `pausing` - Print is paused
* `cancelling` - Print is being cancelled
* `completing` - Print completing (does not fire on a cancelled print)

#### `GCODE_ON_PRINT_STATUS`

Associates a gcode command with a specific status and sets the parameters for
when and how the status event fires.

* `STATUS` - A comma seperated list of status events this command is associated
  with. Passing the value `all` will associate the gcode with all statuses.
* `COMMAND` - The text of the command.
* `ARGS` *(default: `0`)* - Set to `1` to enable passing the following status
  arguments to the macro: `TYPE`, `WHEN`, `LAST_STATUS`, and `NEXT_STATUS`.
  This is useful if calling a custom macro that determines its behavior based
  on the exact details of the state transition.
* `FILTER_ENTER` - An optional list of statuses that, if provided, will prevent
  the command from firing unless the last status matches a status in the list.
* `FILTER_LEAVE` - An optional list of statuses that, if provided, will prevent
  the command from firing unless the next status matches a status in the list.
* `TYPE` *(default: `ENTER`)* - If set to `ENTER` the command is run when
  entering the specified status. If set to `LEAVE` the command is run when
  leaving the specified status. If set to `BOTH` the command is run when
  entering and leaving. The `LEAVE` commands are processed first, followed by
  the `ENTER` commands, all in the order they were originally set.
* `WHEN` *(default: `PRINTING`)* - Set to `PRINTING` to fire the status event
  only when printing, `IDLE` when not printing, and `ALWAYS` for both.

#### Print Status Event Example

Below is a simple example of how to set up a status event config. The calls to
`GCODE_ON_PRINT_STATUS` are placed in the `gcode` of the
`[gcode_macro _km_options]` config section, so that they will be run once at
printer start. When the printer enters the `ready` state at startup the command
will echo *` TYPE=LEAVE WHEN=IDLE LAST_STATUS=none NEXT_STATUS=ready`* to the
console, and when it leaves the `ready` state to begin printing it will echo
*`TYPE=ENTER WHEN=PRINTING LAST_STATUS=ready NEXT_STATUS=homing`*.

```
[gcode_macro _km_options]
# Any options variables declared here
gcode:
  GCODE_ON_PRINT_STATUS STATUS=ready COMMAND="STATUS_TEST" ARGS=1 WHEN=ALWAYS TYPE=BOTH

[gcode_macro status_test]
variable_extruder: 0
gcode:
  M118 STATUS_TEST {rawparams}
```

### Velocity

These are some basic wrappers for Klipper's analogs to some of Marlin's velocity
related commands, such as accelleration, jerk, and linear advance.

#### Marlin Compatibility

* The `M201`, `M203`, `M204`, and `M205` commands are implemented by calling
  Klipper's `SET_VELOCITY_LIMIT` command.
* The `M900` command is implemented by calling Klipper's `SET_PRESSURE_ADVANCE`
  command. The `K` factor is scaled by `variable_pressure_advance_scale`
  *(default: `-1.0`)*. If the scaling value is negative the `M900` command has no
  effect.

## Optional Configs

### Bed Mesh

`BED_MESH_CALIBRATE` and `G29`

Overrides the default `BED_MESH_CALIBRATE` to use `BED_MESH_CALIBRATE_FAST`
instead, and adds the `G29` command.

***Configuration:***

```
[include klipper-macros/optional/bed_mesh.cfg]
```

***Requirements:** A properly configured `bed_mesh` section.*

### LCD Menus

Adds relevant menu items to an LCD display and improves some existing
functionality. See the [customization](#customization) section above for more
information on how to configure specific behaviors.

* Confirmation added for cancelling the print or disabling steppers during a
  print.
* Several temperature menu changes:
  * Up to 10 filaments and their corresponding temperatures can be set via
   `variable_menu_temperature`.
  * Per filament chamber temperature controls are available if a 
    `[heater_generic chamber]` section is configured.
  * The cooldown commands are moved to the top level temperature menu.
* The filament loading commands are replaced with macros that use the lengths
  and speeds specified in `variable_load_length` and `variable_load_speed`,
  which includes a priming phase at the end of the load (controlled via
  `variable_load_priming_length` and `variable_load_priming_speed`).
* [Bed surface](#bed-surface) management is integrated into the setup and tuning
  menus.
* The SD card menu has been streamlined for printing and non-printing modes.
* The setup menu includes host shutdown, host restart, speed, and flow controls.
* You can hide the Octoprint or SD card menus if you don't use them 
  (via `variable_menu_show_octoprint` and `variable_menu_show_sdcard`,
  respectively).

***Configuration:***

```
[include klipper-macros/optional/lcd_menus.cfg]
```

***Requirements:** A properly configured `display` section.*
