# 整体流程分析

## function init(dom, theme, opts){}

```js
var chart = new ECharts(dom, theme, opts);
chart.id = 'ec_' + idBase++;
instances[chart.id] = chart;

modelUtil.setAttribute(dom, DOM_ATTRIBUTE_KEY, chart.id);

enableConnect(chart);

return chart;
```

## function ECharts(dom, theme, opts){}

一些内部管理机制：

- this._chartsViews = [];
- this._chartsMap = {};
- this._componentsViews = [];
- this._componentsMap = {};
- this._coordSysMgr = new CoordinateSystemManager();

### CoordinateSystemManager

实例属性

- create,
- update,
- getCoordinateSystems

静态属性

- register,
- get

var api = this._api = createExtensionAPI(this);

### createExtensionAPI

```js
return zrUtil.extend(new ExtensionAPI(ecInstance), {
    // Inject methods
    getCoordinateSystems: zrUtil.bind(
        coordSysMgr.getCoordinateSystems, coordSysMgr
    ),
    getComponentByElement: function (el) {
        while (el) {
            var modelInfo = el.__ecComponentInfo;
            if (modelInfo != null) {
                return ecInstance._model.getComponent(modelInfo.mainType, modelInfo.index);
            }
            el = el.parent;
        }
    }
});
```

this._scheduler = new Scheduler(this, api, dataProcessorFuncs, visualFuncs);

### Scheduler

内部机制：

- this.ecInstance = ecInstance；
- this.api = api;
- this.unfinished;
- this._allHandlers
- this._stageTaskMap

this._messageCenter = new MessageCenter();

### MessageCenter

```js
function MessageCenter() {
    Eventful.call(this);
}
```

this._pendingActions = [];

## echartsProto.setOption = function (option, notMerge, lazyUpdate)

一些内部管理机制：
var optionManager = new OptionManager(this._api);

### OptionManager

var ecModel = this._model = new GlobalModel();

### GlobalModel


## ecModel.init(null, null, theme, optionManager)

## this._model.setOption(option, optionPreprocessorFuncs)


