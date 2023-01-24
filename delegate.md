
# ➕ 참고사항!

## 요거는 나만의 프로토콜을 만들어주는 방법이랍니다~~?

세미나처럼 이미 있는 프로토콜을 사용한다면 `파랑이`만 참고해주세요!

## iOS14부터 사용할 수 있는 먼가 NEW..~한 친구!

### ✨ ****Diffable DataSource & Compositional Layout 😎****

저는 Diffable Datasource와 Compositional layout을 사용해 CollectionView를 구현했답니다!

요 친구의 장점은 데이터 변경으로 뷰가 업데이트 될 때 예쁜 애니메이션이 함께 나온다는 것 ㅋㅋ

나는 휴학해서 시험기간 같은거 업따?? 하시면 이번 휴식기에 공부해보심 좋을 것 같아요!!

- 참고할만한 튜토리얼
    
    [Modern Collection Views with Compositional Layouts](https://www.raywenderlich.com/5436806-modern-collection-views-with-compositional-layouts)
    
    [iOS Tutorial: Collection View and Diffable Data Source](https://www.raywenderlich.com/8241072-ios-tutorial-collection-view-and-diffable-data-source)
    
- 애플 공식 문서
    
    [Apple Developer Documentation](https://developer.apple.com/documentation/uikit/views_and_controls/collection_views/implementing_modern_collection_views)
    

---

# 진짜 시작합니다!

### 🖼 우선 한 장 정리!

![IMG_100725D1C8AA-1](https://user-images.githubusercontent.com/75439868/214234674-624cf462-f844-4e4b-a94b-ac4f6bc8105d.jpeg)

### 우선 제가 하고 싶은 것은..

![Untitled](https://user-images.githubusercontent.com/75439868/214234709-8ed2a742-b301-4c94-be92-0c6c07a2b387.png)

---

`보내는 뷰컨` : Yellow

`받는 뷰컨` : Blue

## 1.  프로토콜 작성하기 - `보내는 뷰컨`

```swift
protocol StarHandleDelegate {
    func starButtonDidTap(cell: TagAlbumCollectionViewCell)
}
```

프로토콜 이름과 구현하고싶은 함수를 선언해줍시다~ 여기 파라미터 넣은 값을 전달할 수 있어요!

## 2. 딜리게이트 선언 - `보내는 뷰컨`

```swift
var starDelegate: StarHandleDelegate?
```

그리고 머시기 딜리게이트를 만든건지 함.. 알려줘봅시다

## 3. 그래서 데이터 언제 줄건데?

```swift
@IBAction func starButtonDidTap(_ sender: UIButton) {
    self.starDelegate?.starButtonTapped(cell: cell)
}
```

### 좋아요~~ 이제 받는 뷰컨에서 이것저것 함 해봅시다

## 4. 딜리게이트 채택 - `받는 뷰컨`

```swift
class TagViewController: StarHandleDelegate {
}
```

이제 위임받을 뷰컨에 뭘 위임받을건지 적어봅시다!

## 5. 위임자 지정 - `받는 뷰컨`

```swift
albumCell.starDelegate = self
```

어떤 친구가 할 일을 위임받을지 적어줍시다!

## 6. 함수 구현 - `받는 뷰컨`

```swift
func starButtonDidTap(cell: TagAlbumCollectionViewCell) {
    print("✨starButtonDidTap")
}
```

그리고 프로토콜에 선언했던 함수를 구현합니다~~~

# 이게 절대 제일 좋은 방법이 아님을 주의~

## 더 멋진 방법이 있을 수 있음~~~!!✨✨👊😎
