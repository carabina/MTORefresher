# MTORefresher
MTORefresher is a Swift implementation of pull-to-refresh, include pull-down and pull-up. Use 1 line of code can make this. Also you can use Component protocol to custom your own pull-to-refresh Component.



## Basic
### Setup
```Swift
// pull down
let topView: SimpleTopComponent = SimpleTopComponent()
// pull up
let bottomView: SimpleBottomComponent = SimpleBottomComponent()
refresher = tableView
    .mto_refresher()
    .add(topView: topView) { [weak self] in
        self?.reload()
    }
    .add(bottomView: bottomView, enableTap: true) { [weak self] in
        self?.loadMore()
    }
// hide has bottom view
refresher?.canPullUp = false
```

### Trigger
```Swift
// trigger pull down
refresher?.triggerLoad(type: .PullDown)
// trigger pull up
refresher?.triggerLoad(type: .PullUp)
```

### Load Data
```Swift
private func reload() {
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, Int64(0.1*Double(NSEC_PER_SEC))), dispatch_get_main_queue()) {
        self.count = 2
        self.tableView.reloadData()
        // stop loading
        self.refresher?.stopLoad()
        // show bottom view
        self.refresher?.canPullUp = true
        self.refresher?.hasMore = true
    }
}
    
private func loadMore() {
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, Int64(0.3*Double(NSEC_PER_SEC))), dispatch_get_main_queue()) {
        self.count += 20
        self.tableView.reloadData()
        // stop loading
        self.refresher?.stopLoad()
        let hasMore = (rand()%2) == 0 ? true : false
        // has more data
        self.refresher?.hasMore = hasMore
    }
}
```

## Custom Component

Just create subview that confirms to Componet protocol. See `SimpleTopComponent` and `SimpleBottomComponent` for more! It's very easy to extend.