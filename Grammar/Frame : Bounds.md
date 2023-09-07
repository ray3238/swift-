# Frame / Bounds

------

- **Frame**

  superView의 좌표계에서 뷰의 위치와 크기를 나타내는 사각형 형태의 틀

  frame이 나타내는 origin(x, y) 좌표는

  **Super View의 원점을 (0, 0)으로 놓고**

  **원점으로부터 얼마나 떨어져 있는지**

  ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/174967fb-1dab-43ea-906d-972ee62e9e66/Untitled.png)

  **firstView의 frame의 origin은**

  **(20, 50)**

  ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/cb50eeaf-c211-4cf0-8db7-767b5a61a388/Untitled.png)

  **secondView의 frame의 origin은**

  **(10, 10)**

  SIZE를 젤 때는

  ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/9835a25e-c019-4761-b829-6406cc72f305/Untitled.png)

  도형이 차지하는 만큼이 아니라 view의 영역 만큼.

- **Bounds**

  자체적인 좌표계 내에서 위치와 크기를 표현

  **자신의 원점을 (0, 0)으로 놓음**

  ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/d51b45a4-76e0-4844-b4a1-18b1899fd6bf/Untitled.png)

  ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3a43468a-0f71-4f8b-87a5-c4d40798ae72/45142e7d-af7f-4959-9993-1a71ebd8f1e6/Untitled.png)

  Bounds는 Frame과 다르게 도형이 차지하는 만큼만 공간을 차지

정리

|                           | Frame                          | Bounds         |
| ------------------------- | ------------------------------ | -------------- |
| origin (x, y) 기준점      | Super View의 좌표계            | 자신이 좌표계  |
| size (width, height) 기준 | View 영역을 모두 감싸는 사각형 | View 영역 자체 |