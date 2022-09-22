# combine_execute_one_by_one

```swift
            self.mixRepository
                .deselect(uid: uid)
                .retry(3)
                .compactMap({ [weak self] in
                    self?.mixRepository
                        .save(mix: mix)
                        .retry(3)
                })
                .switchToLatest()
                .sink { [weak self] result in
                    switch result {
                    case .failure(let error):
                        self?.state = .error(.mixRepositoryError(error))
                    default: break
                    }
                } receiveValue: { [weak self] mix in
                    self?.state = .success(mix)
                }
                .store(in: &subscriptions)
```
