注意，这个代码假设你有 `checkHashExists` 和 `saveHashMapping` 这两个函数来处理哈希值到文件路径的映射。这需要你有一个数据库或者内存中的数据结构来存储这个映射。具体的实现方式取决于你的应用的需求和环境。

```go
import (
	"crypto/sha256"
	"encoding/hex"
	"image"
	"image/png"
	"io"
	"net/http"
	"os"
	"strings"

	"github.com/nfnt/resize"
)

func UploadLocal(writer http.ResponseWriter, request *http.Request) {
	//获得上传源文件
	srcFile, head, err := request.FormFile("file")
	if err != nil {
		util.RespFail(writer, err.Error())
		return
	}
	defer srcFile.Close()

	//计算文件的哈希值
	hash := sha256.New()
	if _, err := io.Copy(hash, srcFile); err != nil {
		util.RespFail(writer, err.Error())
		return
	}
	hashValue := hex.EncodeToString(hash.Sum(nil))

	//检查哈希值是否已存在，如果存在则不再进行后续的保存和压缩操作
	//这需要你有一个哈希值到文件路径的映射，可以使用数据库或者内存中的数据结构来存储这个映射
	if existingFile, exists := checkHashExists(hashValue); exists {
		util.RespOk(writer, existingFile, "")
		return
	}

	// Reset the file read pointer to the beginning after the hash calculation
	srcFile.Seek(0, 0)

	//读取图片并压缩
	srcImage, _, err := image.Decode(srcFile)
	if err != nil {
		util.RespFail(writer, err.Error())
		return
	}
	resizedImage := resize.Resize(1000, 0, srcImage, resize.Lanczos3)

	//创建一个新的文件
	filename := fmt.Sprintf("%d%s%s", time.Now().Unix(), util.GenRandomStr(32), ".png")
	filepath := "./resource/" + filename
	dstfile, err := os.Create(filepath)
	if err != nil {
		util.RespFail(writer, err.Error())
		return
	}
	defer dstfile.Close()

	//将压缩后的图片保存到新文件
	png.Encode(dstfile, resizedImage)

	//保存哈希值到文件路径的映射
	saveHashMapping(hashValue, filepath)

	util.RespOk(writer, filepath, "")
}

```

