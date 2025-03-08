# Deadline Cloud Plugins Monorepo example

## Purpose
The purpose of this repository is to show the interactions between `deadline-cloud`, `deadline-cloud-for-maya` and a new repo `deadline-cloud-for-arnold-standalone` with our new plugin system.

## Overview
There are multi-dcc submitter concepts that are not currently served by the existing Deadline Cloud DCC submitter system.
Am obvious first example is the Arnold render engine.
In Deadline 10, users could submit an Arnold standalone job from several DCCs (3dsMax, Maya, Houdini, etc) and utilize a single plugin on the render node that would render the `.ass` file.

In Deadline Cloud, no such concept exists. If the team wants to add support for Arnold in Maya, any code they write cannot be utilized in 3dsMax or Houdini natively.
This proposal provides a way to utilize code across DCCs.

## Future Work

### Plugin Job Functions
Additional ideas in this vein would expose a `arnold_util.job` module that can expose `get_job_template` and `get_parameter_values` to aid in reducing code duplication within the DCC specific plugin.
This would require some standardized interface to allow the DCC-agnostic plugin to query for `render_layers`, `cameras`,plus likely other information.
That is out of the scope of this PR at the moment.

# Implementation Detail
See [Implementation.md](Implementation.md)

## Arnold Standalone Submitter
See [Arnold-Implementation.md](Arnold-Implementation.md)
