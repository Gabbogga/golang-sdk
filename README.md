# Novita AI Golang SDK

This SDK is based on the official [API documentation](https://docs.novita.ai/).

**Join our discord server for help**

[![](https://dcbadge.vercel.app/api/server/Mqx7nWYzDF)](https://discord.gg/Mqx7nWYzDF)

## Installation

```bash
go get -u github.com/Gabbogga/golang-sdk
```

## Quick Start

**Get api key refer to [https://novita.ai/get-started/](https://novita.ai/get-started/)**
Gabbogga
```golang
package main

import (
	"context"
	"fmt"
	"time"

	"github.com/Gabbogga/golang-sdk/request"
	"github.com/Gabbogga/golang-sdk/types"
)

func main() {
	// Get your API key refer to https://novita.ai/get-started/ .
	const apiKey = "Your-API-Key"
	client, err := request.NewClient(apiKey)
	if err != nil {
		fmt.Printf("new client failed, %v\n", err)
		return
	}
	ctx, cancel := context.WithTimeout(context.Background(), time.Minute*3)
	defer cancel()
	txt2ImgReq := types.NewTxt2ImgRequest("a dog flying in the sky", "", "AnythingV5_v5PrtRE.safetensors")
	res, err := client.SyncTxt2img(ctx, txt2ImgReq,
		request.WithSaveImage("out", 0777, func(taskId string, fileIndex int, fileName string) string {
			return "test_txt2img_sync.png"
		}))
	if err != nil {
		fmt.Printf("generate image failed, %v\n", err)
		return
	}
	for _, s3Url := range res.Data.Imgs {
		fmt.Printf("generate image url: %v\n", s3Url)
	}
}
```

## Examples

### Txt2Img with LoRA

Refer to [./example/lora/main.go](./example/lora/main.go)

### Model Search

Refer to [./example/model_search/main.go](./example/model_search/main.go)

### ControlNet QRCode

Refer to [./example/qrcode/main.go](./example/qrcode/main.go)

## Testing

```
API_KEY=<your-key> go test ./...
```