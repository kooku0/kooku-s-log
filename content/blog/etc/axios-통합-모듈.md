---
title: axios 통합 모듈
date: 2019-12-30 16:12:99
category: etc
---

>  이 글을 어디에 쓸지, 제목은 어떻게 해야할지 고민이 많았다. 결국 etc 카테고리에 넣었는데 잘 한건지는.. 

한달전? 쯤 당근마켓에서 채용공고를 한 적이 있다. 과제를 수행하는 것이였는데 tamplate 는 주어지고, 비어있는 부분의 코드를 짜는 것이였다. 이때 받은  템플릿코드가 너무 좋아서 여기에 포스팅 해보려고 한다.

특히 마음에 들었던 부분은 axios를 이용한 api request, response 부분이였다.

폴더구조는 다음과 같았다.

```shell
|services
|--AuthService.ts
|--ProductService.ts
|--types.ts
```

```typescript
// AuthService.ts
import axios from 'axios';
import { ApiResponse } from '~services/types';

export type LoginResponseDto = {
  token: string;
  id: number;
}

export type LoginSignupRequestDto = {
  email: string;
  password: string;
};

export type AuthResponseDto = {
  id: string;
  email: string;
  password: string;
}

const API_HOST = process.env.API_HOST || 'http://localhost:5000/api';

class AuthService {
  async login(body: LoginSignupRequestDto): Promise<ApiResponse<LoginResponseDto>> {
    return axios.post(`${API_HOST}/auth/login`, body);
  }

  async signUp(body: LoginSignupRequestDto): Promise<ApiResponse<AuthResponseDto>> {
    return axios.post(`${API_HOST}/auth/signup`, body);
  }
}

export default AuthService;
```

```typescript
// ProductService.ts
import axios from 'axios';
import { ApiResponse } from '~services/types';
import AuthStore from '~stores/auth/AuthStore';

export type ProductRegistrationDto = {
  userId?: string;
  image: File;
  category: number;
  title: string;
  description: string;
  price: number;
}

export type ProductDto = {
  id: number;
  userId: string;
  title: string;
  image: string;
  category: number;
  description: string;
  price: number;
  createdAt: string;
  updatedAt: string;
}

const API_HOST = process.env.API_HOST || 'http://localhost:5000/api';

class ProductService {

  constructor(private authStore: AuthStore) {
  }

  async registration(body: ProductRegistrationDto): Promise<ApiResponse<ProductDto>> {
    if (this.authStore.auth == null) {
      throw new Error('need to login!');
    }
    const formData = new FormData();
    formData.append('image', body.image);
    formData.append('userId', String(this.authStore.auth.id));
    formData.append('category', String(body.category));
    formData.append('title', body.title);
    formData.append('description', body.description);
    formData.append('price', String(body.price));

    return axios.post<ProductRegistrationDto, ApiResponse<ProductDto>>(`${API_HOST}/products`, formData, {
      headers: {'Content-Type': 'multipart/form-data' }
    });
  }

  async getAll(): Promise<ApiResponse<ProductDto[]>> {
    return axios.get(`${API_HOST}/products`);
  }

  async getById(id: string): Promise<ApiResponse<ProductDto>> {
    return axios.get(`${API_HOST}/products/${id}`);
  }

}

export default ProductService;
```

```typescript
// types.ts
import { AxiosResponse } from 'axios';

export interface Response<T> {
  data: T,
  msg?: string
}

export type ApiResponse<T> = AxiosResponse<Response<T>>
```

`types.ts` 파일을 보면 알겠지만 typescript의 템플릿을 이용하여 response를 구성하였다. 그리고 data의 interface는 각 서비스 파일에서 만들어 넣어주었다. 진짜 깔끔 그 자체

여기에서 경로는 다음과 같이 `constans.ts`파일로 따로 뺐는데 `enum`을 이용해서 이쁘게 만들었더라.. 

진짜 많은 걸 배울 수 있는 코드였다.

```typescript
// constants.ts
export enum STORES {
  AUTH_STORE = 'authStore',
  PRODUCTS_STORE = 'productsStore',
}

export enum PAGE_PATHS {
  SIGNUP = '/signup',
  SIGNIN = '/signin',
  PRODUCT_LISTS = '/products',
  PRODUCT_CAR_CATEGORY_LISTS = '/car-products',
  PRODUCT = '/products',
  PRODUCT_REGISTRATION = '/products-registration',
}
```

