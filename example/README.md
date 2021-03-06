# LoadAny Example

A example Flutter application of LoadAny.

[中文说明](https://github.com/yy1300326388/loadany/tree/master/example/README_CN.md)

[![Codemagic build status](https://api.codemagic.io/apps/5d561d3a6a6c3600097b43a6/5d561d3a6a6c3600097b43a5/status_badge.svg)](https://codemagic.io/apps/5d561d3a6a6c3600097b43a6/5d561d3a6a6c3600097b43a5/latest_build)

## Getting Started

```Dart
import 'package:loadany/loadany.dart';
```

```Dart
LoadStatus status = LoadStatus.normal;

LoadAny(
  onLoadMore: getLoadMore,
  status: status,
  child: CustomScrollView(
    slivers: <Widget>[
      SliverGrid(...),
      SliverList(...),
    ],
  ),
)
```
```Dart
/// Load More Get Data
Future<void> getLoadMore() async {
    setState(() {
      status = LoadStatus.loading;
    });
    Timer.periodic(Duration(milliseconds: 5000), (Timer timer) {
      timer.cancel();
      int length = list.length;
      for (var i = 1; i < 11; ++i) {
        list.add(length + i);
      }
    
      if (length > 80) {
        status = LoadStatus.completed;
      } else if (length >= 50 && length < 70) {
        status = LoadStatus.error;
      } else {
        status = LoadStatus.normal;
      }
      setState(() {});
    });
}
```

### Set LoadMore Footer height

```Dart
LoadAny(
  onLoadMore: getLoadMore,
  status: status,
  footerHeight: 40, // Add
  child: CustomScrollView(
    slivers: <Widget>[
      SliverGrid(...),
      SliverList(...),
    ],
  ),
)
```

### Set the feed trigger flow bottom trigger timing height

```Dart
LoadAny(
  onLoadMore: getLoadMore,
  status: status,
  endLoadMore: false, // Add
  bottomTriggerDistance: 200, // Add
  child: CustomScrollView(
    slivers: <Widget>[
      SliverGrid(...),
      SliverList(...),
    ],
  ),
)
```

### Custom status text

```Dart
LoadAny(
  onLoadMore: getLoadMore,
  status: status,
  loadingMsg: 'Loading...', // Add
  errorMsg: 'Error', // Add
  finishMsg: 'Finish', // Add 
  child: CustomScrollView(
    slivers: <Widget>[
      SliverGrid(...),
      SliverList(...),
    ],
  ),
)
```

### Customize LoadMore Footer styles

<img src="https://raw.githubusercontent.com/yy1300326388/loadany/develop/example/images/Simulator%20Screen%20Shot%2005.png" width="220"/>

```Dart
LoadAny(
  onLoadMore: getLoadMore,
  status: status,
  loadMoreBuilder: (BuildContext context, LoadStatus status) {
    if (status == LoadStatus.loading) {
      return Container(
        height: 40,
        child: Text('Loading'),
        color: Colors.green,
        alignment: Alignment.center,
      );
    }
    return null;
  },
  child: CustomScrollView(
    slivers: <Widget>[
      SliverGrid(...),
      SliverList(...),
    ],
  ),
)
```

### Add RefreshIndicator to refresh

<img src="https://raw.githubusercontent.com/yy1300326388/loadany/develop/example/images/Kapture%2004.gif" width="220"/>

```Dart
RefreshIndicator(
    onRefresh: () async {
      if(status==LoadStatus.normal) {
        await GetData();
        ...
        setState(() {});
      }
    },
    child: LoadAny(
      onLoadMore: getLoadMore,
      status: status,
      child: CustomScrollView(
        slivers: <Widget>[
          SliverAppBar(...),
          SliverGrid(...),
          SliverList(...),
        ],
      ),
    ),
)
```



