---
layout: post
title: "Create a Custom CollectionView for a macOS App in Swift"
categories:
  - Tutorials
tags:
  - swift
  - coding
  - macOS

last_modified_at: 
excerpt_separator: <!-- more -->
---

1. Register CustomCollectionViewItem in the CustomViewController
2. Adopt NSCollectionViewDataSource in CustomViewController
3. Override `loadView()` in CustomCollectionViewItem

<!-- more -->

```swift
import Cocoa

class CustomViewController: NSViewController {
    @IBOutlet weak var collectionView: NSCollectionView!

    override func viewDidLoad() {
        self.collectionView.register(CustomCollectionViewItem.self, forItemWithIdentifier: NSUserInterfaceItemIdentifier("CustomCollectionViewItem"))
    }
}

extension CustomViewController: NSCollectionViewDataSource {
    func collectionView(_ collectionView: NSCollectionView, numberOfItemsInSection section: Int) -> Int {
        return dataSource.count
    }
    
    func collectionView(_ collectionView: NSCollectionView, itemForRepresentedObjectAt indexPath: IndexPath) -> NSCollectionViewItem {
        let item = self.collectionView.makeItem(withIdentifier: NSUserInterfaceItemIdentifier("CustomCollectionViewItem"), for: indexPath)
        return item
    }
}

class CustomCollectionViewItem: NSCollectionViewItem {
    override func loadView() {
    }
}

```

