Алгоритм кэширования 2Q (Two-Queue Cache) является улучшенной версией алгоритма LRU (Least Recently Used). В основе его работы лежит использование двух очередей для разделения данных на "часто используемые" и "недавно добавленные". Это позволяет улучшить производительность кэширования за счет снижения количества "промахов" кэша (cache miss), когда требуемые данные отсутствуют в кэше.

### Концепция 2Q Cache

2Q алгоритм разделяет кэш на три основные части:
1. **IN (FIFO queue)** - очередь для новых или недавно используемых элементов.
2. **MAIN (LRU queue)** - основная очередь для элементов, которые были подтверждены как "часто используемые".
3. **OUT (FIFO queue)** - очередь для элементов, которые были исключены из IN, но ещё могут вернуться в кэш.

### Основные шаги алгоритма:

- При доступе к элементу сначала проверяется основная очередь MAIN.
- Если элемент присутствует в MAIN, он обрабатывается как попадание (hit) и перемещается в начало очереди MAIN (по принципу LRU).
- Если элемента нет в MAIN, проверяется очередь IN.
  - Если элемент находится в IN, это также считается попаданием, но элемент остается в IN.
  - Если элемента нет в IN, проверяется очередь OUT.
    - Если элемент находится в OUT, это означает, что его следует перенести в MAIN.
    - Если элемента нет и в OUT, это промах, и элемент добавляется в IN.

- При добавлении в IN, если очередь переполнена, наиболее старый элемент из IN перемещается в OUT.
- Если OUT переполняется, наиболее старый элемент удаляется из OUT.

### Пример реализации на Swift

Ниже представлена базовая реализация алгоритма 2Q Cache на Swift:

```swift
import Foundation

class TwoQueueCache<Key: Hashable, Value> {
    private struct CachePayload {
        let key: Key
        let value: Value
    }

    private let mainCapacity: Int
    private let inCapacity: Int
    private let outCapacity: Int

    private var mainCache = [Key: CachePayload]()
    private var inQueue = [CachePayload]()
    private var outQueue = [Key]()

    init(mainCapacity: Int, inCapacity: Int, outCapacity: Int) {
        self.mainCapacity = mainCapacity
        self.inCapacity = inCapacity
        self.outCapacity = outCapacity
    }

    func get(_ key: Key) -> Value? {
        if let payload = mainCache[key] {
            // Move to front in MAIN (LRU logic)
            mainCache[key] = payload
            return payload.value
        }

        if let index = inQueue.firstIndex(where: { $0.key == key }) {
            return inQueue[index].value
        }

        if let outIndex = outQueue.firstIndex(where: { $0 == key }) {
            // Move from OUT to MAIN
            moveToMain(fromOutIndex: outIndex)
            return mainCache[key]?.value
        }

        return nil
    }

    func set(_ value: Value, for key: Key) {
        let newPayload = CachePayload(key: key, value: value)

        if mainCache[key] != nil || inQueue.contains(where: { $0.key == key }) {
            return // Element already in cache, do nothing
        }

        if outQueue.contains(key) {
            moveToMain(fromOutIndex: outQueue.firstIndex(of: key)!)
            return
        }

        // Add to IN
        if inQueue.count >= inCapacity {
            moveFromInToOut()
        }
        inQueue.insert(newPayload, at: 0)
    }

    private func moveToMain(fromOutIndex index: Int

) {
        let key = outQueue.remove(at: index)
        if mainCache.count >= mainCapacity {
            mainCache.remove(at: mainCache.keys.index(mainCache.startIndex, offsetBy: 0))
        }
        let value = inQueue.remove(at: inQueue.firstIndex(where: { $0.key == key })!)
        mainCache[key] = value
    }

    private func moveFromInToOut() {
        let oldKey = inQueue.removeLast().key
        if outQueue.count >= outCapacity {
            outQueue.removeLast()
        }
        outQueue.insert(oldKey, at: 0)
    }
}
```

Эта реализация предоставляет базовый функционал 2Q кэша, но может быть дополнена и оптимизирована в зависимости от конкретных требований и условий использования.

```Swift
var viewModels: [MainViewModel] = []
var detailModels: [DetailViewModel] = []

func didLoadData(with response: iTunesResponse) {
    self.interactor.loadImage(with: iTunesResponse)
}

private func configureView(with: viewModels, with: detailModels) {					self.view.configure(with: viewModels, with: detailModels)
}

final class MainInteractor {
    weak var output: MainInteractorOutput?
}

extension MainInteractor: MainInteractorInput {
    func loadData(searchText: String) {
        SearchNetworkManager.shared.loadData(searchText: searchText){ result in
            switch result {
            case .success(let success):
                self.output?.didRecieve(result: .success(success))
            case .failure(let failure):
                self.output?.didRecieve(result: .failure(failure))
            }
        }
    }

	func loadImage(with response: iTunesResponse) {
		let dispatchGroup = DispatchGroup()
        
        for item in response.results {
            dispatchGroup.enter()
            
            SearchNetworkManager.shared.downloadImage(from: item.artworkUrl100) { result in
                defer {
                    dispatchGroup.leave()
                }
                
                guard let image = result else {
                    return
                }
                
                let viewModel = MainViewModel(
                    title: item.trackName ?? item.collectionName ?? "Name N/F",
                    contentType: item.wrapperType,
                    contentImage: image
                )
                viewModels.append(viewModel)
                
                guard let trackViewUrl = item.trackViewUrl else { return }
                let detailModel = DetailViewModel(
                    title: item.trackName ?? item.collectionName ?? "Name N/F",
                    authorName: item.artistName,
                    contentType: item.wrapperType,
                    trackViewUrl: trackViewUrl,
                    description: item.longDescription ?? "Description N/F",
                    artictLinkUrl: NetworkConstants.baseLookupURl + String(item.artistId),
                    contentImage: image
                )
                detailModels.append(detailModel)
            }
            
            dispatchGroup.notify(queue: .main) {
                self.presenter?.configureView(with: viewModels, with: detailModels)
            }
        }
    }
	}
}

----------------------

protocol MainViewOutput: AnyObject {
    func didChangeSearchText(searchText: String)
    func didSearchBarBookmarkButtonClicked(isUsingDefaultLimit: Bool)
    func didLoadView()
    func didTapOnCell(model: DetailViewModel)
}

protocol MainViewInput: AnyObject {
    func configure(with model: [MainViewModel], with detailModel: [DetailViewModel])
}

protocol MainInteractorInput: AnyObject {
    func loadData(searchText: String)
    func loadImage(with response: iTunesResponse)
}

protocol MainInteractorOutput: AnyObject {
    func didRecieve(result: Result<iTunesResponse, Error>)
}

protocol MainRouterInput: AnyObject {
    func openDetailModule(with model: DetailViewModel)
    func openErrorAlert(with failure: String)
}

protocol MainRouterOutput: AnyObject {
    
}
```