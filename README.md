# golang\_documentation

Mutex (Mutual Exclusion) adalah salah satu mekanisme sinkronisasi di Go (Golang) yang digunakan untuk melindungi akses ke sumber daya yang dibagikan antara beberapa goroutine. Mutex memastikan bahwa hanya satu goroutine yang dapat mengakses atau memodifikasi sumber daya tersebut pada satu waktu, sehingga mencegah kondisi balapan (race conditions).

### Konsep Dasar Mutex

1. **Exclusive Locking**: Mutex memberikan akses eksklusif ke suatu sumber daya. Ketika satu goroutine mengunci (lock) mutex, goroutine lain yang mencoba mengunci mutex yang sama akan diblokir hingga mutex tersebut dilepas (unlock) oleh goroutine yang menguncinya.
2. **Deadlock Prevention**: Mutex membantu mencegah deadlock dengan memastikan bahwa hanya satu goroutine yang memiliki akses ke sumber daya kritis pada satu waktu. Namun, penggunaan mutex yang tidak hati-hati dapat menyebabkan deadlock, jadi sangat penting untuk selalu melepas mutex yang telah dikunci.

### Penggunaan Mutex pada Go

````go
```go
package main

import (
	"fmt"
	"sync"
)

var (
	counter int
	mu      sync.Mutex
	wg      sync.WaitGroup
)

func increment() {
	defer wg.Done()
	mu.Lock()
	counter++
	mu.Unlock()
}

func main() {
	numGoroutines := 100

	wg.Add(numGoroutines)
	for i := 0; i < numGoroutines; i++ {
		go increment()
	}
	wg.Wait()

	fmt.Println("Final Counter:", counter)
}
```
````

* `var mu sync.Mutex` mendeklarasikan mutex, dan `counter` adalah variabel bersama yang dilindungi oleh mutex.
* Fungsi `increment` mengunci mutex sebelum mengakses dan memodifikasi `counter`, dan kemudian melepas mutex setelah selesai.
* Di fungsi `main`, kita memulai beberapa goroutine untuk menjalankan fungsi `increment`. `sync.WaitGroup` digunakan untuk menunggu semua goroutine selesai sebelum mencetak nilai akhir `counter`.

### Kapan Menggunakan Mutex?

1. **Menghindari Race Conditions.** Gunakan mutex saat Anda memiliki beberapa goroutine yang mengakses atau memodifikasi sumber daya bersama, seperti variabel global atau data yang dibagikan.
2. **Pengelolaan Sumber Daya.** Gunakan mutex untuk mengelola akses ke sumber daya yang tidak dapat diakses secara bersamaan, seperti file, koneksi jaringan, atau struktur data kompleks.

### Kesimpulan

Mutex adalah alat penting dalam Go untuk mengelola akses bersamaan ke sumber daya bersama. Dengan mengunci dan melepas mutex dengan benar, Anda dapat mencegah kondisi balapan dan memastikan keamanan thread dalam program konkuren. Namun, penggunaan mutex harus dilakukan dengan hati-hati untuk menghindari deadlock dan memastikan bahwa mutex selalu dilepas setelah dikunci.
