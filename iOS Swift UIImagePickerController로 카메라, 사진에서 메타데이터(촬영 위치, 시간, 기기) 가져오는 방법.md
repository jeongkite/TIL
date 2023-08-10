### 내가 할 수 있었던 것

- 앱 내에서 `카메라`로 촬영한 사진에서 촬영 `기기`, `시간` 가져오기
- `갤러리`에서 선택한 사진에서 촬영 `위치`, `시간` 가져오기

### → 할 수 없었던 것

- 카메라로 찍은 사진에서 바로 위치 가져오기, 갤러리에서 선택한 사진을 통해 촬영 기기 가져오기

내가 몰랐던 더 좋은 방법이 있었을수도..😇

# 사진에서 데이터 가져오기

### 꼭! 필요한 것!!!

- 사진 `.readWrite` 권한 명시적으로 요청하기
  - 이게 없으면 이미지만 불러오고 데이터는 비어있다.
  - 요청하는 방법
    ```swift
    PHPhotoLibrary.requestAuthorization(for: .readWrite){ status in
        switch status {
        case .notDetermined:
            print("사진 권한 요청 결과 : notDetermined")
        case .restricted:
            print("사진 권한 요청 결과 : restricted")
        case .denied:
            print("사진 권한 요청 결과 : denied")
        case .authorized:
            print("사진 권한 요청 결과 : authorized")
        case .limited:
            print("사진 권한 요청 결과 : limited")
        @unknown default:
            fatalError()
        }
    }
    ```

## 데이터 가져오는 방법

- 먼저 UIImagePickerController를 만들어서 채택해줘야 한다.

```swift
class ViewController: UIViewController {
	private let imagePicker = UIImagePickerController()

	override func viewDidLoad() {
		imagePicker.delegate = self
	}
}
```

- 갤러리를 열고 싶을 때, 카메라를 열고 싶을 때 : 각각의 이벤트가 발생한 경우 imagePicker 열어주기
  아래 내용(`sourceType`) 외에도 더 많은 옵션(라이브포토 허용할지, 몇개까지 선택 가능하게 할건지 등..)이 있으니 검색 바람.

```swift
// 갤러리를 열고 싶다면??
imagePicker.sourceType = .photoLibrary
present(imagePicker, animated: true)

// 카메라를 열고 싶다면??
imagePicker.sourceType = .camera
present(imagePicker, animated: true)
```

- 채택해주면 당연히 UIImagePickerControllerDelegate도 상속받아서 필요한 함수도 적어줘야겠죠잉~?

```swift
// MARK: UIImagePickerControllerDelegate
extension ViewController: UIImagePickerControllerDelegate {
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        if let image = info[.originalImage] as? UIImage {
						// 갤러리에서 이미지를 가져온 경우 phAsset 이용
            if let asset = info[.phAsset] as? PHAsset {
                viewModel.createMaterial(image: image, asset: asset)
						// 카메라를 통해 이미지를 가져온 경우 mediaMetadata 이용
            } else if let dictInfo = info[.mediaMetadata] as? [String: Any] {
                viewModel.createMaterial(image: image, metadata: dictInfo)
            } else {
                viewModel.createMaterial(image: image)
            }
            print("✅ 이미지 가져오기 성공!\nimage: ", image)
        } else {
            self.showErrorAlert(reason: "🚫 이미지를 가져오는데 문제가 발생했습니다.")
        }
        dismiss(animated: true)
    }
}
```

### `info[.phAsset]` 이용하는 경우

```swift
// 시간
if let unFormattedDate = asset.creationDate {
    materialContent.takenAt = formatter.string(from: unFormattedDate)
}

// 위치
if let location = asset.location {
    materialContent.latitude = String(location.coordinate.latitude)
    materialContent.longitude = String(location.coordinate.longitude)
}
```

### `info[.mediaMetadata]` 를 이용하는 경우

- 타입이 지정되지 않은 데이터가 배열로 들어있음
  필요한 데이터만 예쁘게 빼줍시다

```swift
func createMaterial(image: UIImage, asset: PHAsset? = nil, metadata: [String: Any]? = nil) {
    if let tiffInfo = metadata["{TIFF}"] as? [String: Any] {
				// 기기
        materialContent.device = tiffInfo["Model"] as! String

				// 시간
        materialContent.takenAt = formatTIFFString(data: tiffInfo["DateTime"])
    }
}
```

- 기본적으로 다 optional이라 바인딩 필요합니다
