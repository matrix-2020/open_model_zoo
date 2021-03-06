# How to configure MxNet launcher

For enabling MxNet launcher you need to add `framework: mxnet` in launchers section of your configuration file and provide following parameters:

* `device` - specifies which device will be used for infer (`cpu`, `gpu_0` and so on).
* `model`- path to params file specifying the numeric arrays used in the network.
* `adapter` - approach how raw output will be converted to representation of dataset problem, some adapters can be specific to framework. You can find detailed instruction how to use adapters [here](../adapters/README.md).

You also should specify all inputs for your model and provide their shapes, using specific parameter: `inputs`.
Each input description should has following info:
  * `name` - input layer name in network
  * `type` - type of input values, it has impact on filling policy. Available options:
    * `CONST_INPUT` - input will be filled using constant provided in config. It also requires to provide `value`.
    * `IMAGE_INFO` - specific key for setting information about input shape to layer (used in Faster RCNN based topologies). You do not need provide `value`, because it will be calculated in runtime. Format value is `Nx[H, W, S]`, where `N` is batch size, `H` - original image height, `W` - original image width, `S` - scale of original image (default 1).
    * `INPUT` - network input for main data stream (e. g. images). If you have several data inputs, you should provide regular expression for identifier as `value` for specifying which one data should be provided in specific input.
  * `shape` - shape of input layer described as comma-separated of all dimensions size except batch size. 
    Optionally you can determine `layout` in case when your model was trained with non-standard data layout (For MxNet default layout is `NCHW`).
You also can specify batch size for your model using `batch`.

MxNet launcher config example:

```yml
launchers:
  - framework: mxnet
    device: CPU
    model: path_to_model/alexnet-0000.params
    inputs:
      - name: 'input'
        type: INPUT
        shape: 3, 32, 32
    adapter: classification
```
