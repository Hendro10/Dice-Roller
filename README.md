# Dice-Roller
Membuat aplikasi Dice Roller interaktif

Tentang codelab ini
subjectTerakhir diperbarui Sep 22, 2023
account_circleDitulis oleh Tim Pelatihan Google Developers
1. Sebelum memulai
Dalam codelab ini, Anda akan membuat aplikasi Dice Roller interaktif yang memungkinkan pengguna melempar dadu dengan mengetuk composable Button. Hasil lemparan ditampilkan dengan composable Image di layar.

Anda menggunakan Jetpack Compose dengan Kotlin untuk membuat tata letak aplikasi, lalu menulis logika bisnis untuk menangani peristiwa yang terjadi saat composable Button diketuk.

Prasyarat
Mampu membuat dan menjalankan aplikasi Compose dasar di Android Studio.
Memahami cara menggunakan composable Text dalam aplikasi.
Memahami cara mengekstrak teks ke resource string untuk memudahkan penerjemahan aplikasi dan menggunakan kembali string tersebut.
Memahami dasar-dasar pemrograman Kotlin.
Yang akan Anda pelajari
Cara menambahkan composable Button ke aplikasi Android dengan Compose.
Cara menambahkan perilaku ke composable Button di aplikasi Android dengan Compose.
Cara membuka dan mengubah kode Activity untuk aplikasi Android.
Yang akan Anda bangun
Aplikasi Android interaktif bernama Dice Roller yang memungkinkan pengguna melempar dadu dan menampilkan hasil lemparannya.
Yang akan Anda butuhkan
Komputer yang dilengkapi Android Studio.
Tampilan aplikasi akan terlihat seperti berikut saat Anda menyelesaikan codelab ini:

524ad07a9b61f729.png

2. Menetapkan dasar pengukuran
Membuat project
Di Android Studio, klik File > New > New Project.
Dalam dialog New Project, pilih Empty Activity, lalu klik Next.
Dialog akan muncul dengan daftar template project Android. Setiap template menampilkan gambar template dasar beserta nama template. 

Di kolom Name, masukkan Dice Roller.
Di kolom Minimum SDK, pilih API level minimum 24 (Nougat) dari menu, lalu klik Finish.
f59332f7db364338.png

Catatan: Jalur yang dilihat dalam gambar tidak valid dan hanya digunakan untuk contoh. Jalur yang tidak valid akan menghasilkan peringatan yang bertuliskan "The path ‘/Users' is not writable. Please choose a new location." Peringatan ini tidak akan muncul jika jalur valid dan dapat diabaikan pada gambar.

3. Membuat infrastruktur tata letak
Melihat pratinjau project
Untuk melihat pratinjau project:

Klik Build & Refresh di panel Split atau Design.
c367df1b2c82b224.png

Sekarang Anda akan melihat pratinjau di panel Design. Jangan khawatir jika project terlihat kecil karena akan berubah saat Anda mengubah tata letak.

c968f0707e081b8f.png

Mengubah struktur kode contoh
Anda perlu mengubah beberapa kode yang telah dibuat agar lebih mirip dengan tema aplikasi dadu di atas.

Seperti yang Anda lihat di screenshot aplikasi akhir, ada gambar dadu dan tombol untuk melemparnya. Anda akan menyusun fungsi composable untuk mencerminkan arsitektur ini.

Untuk menyusun ulang kode contoh:

Hapus fungsi DefaultPreview().
Buat fungsi DiceWithButtonAndImage() dengan anotasi @Composable.
Fungsi composable ini mewakili komponen UI tata letak dan juga menampung logika klik tombol dan tampilan gambar.

Hapus fungsi Greeting(name: String).
Buat fungsi DiceRollerApp() dengan anotasi @Preview dan @Composable.
Aplikasi ini hanya terdiri dari tombol dan gambar. Jadi, anggap fungsi composable ini sebagai aplikasi itu sendiri. Itulah alasannya fungsi ini disebut fungsi DiceRollerApp().

MainActivity.kt


@Preview
@Composable
fun DiceRollerApp() {

}

@Composable
fun DiceWithButtonAndImage() {

}
Panggilan ke Greeting("Android") dalam isi lambda DiceRollerTheme() disorot dengan warna merah karena Anda menghapus fungsi Greeting(). Ini karena compiler tidak dapat menemukan referensi ke fungsi tersebut lagi.

Hapus semua kode di dalam lambda setContent{} yang ditemukan dalam metode onCreate().
Di bagian lambda setContent{}, panggil lambda DiceRollerTheme{}, lalu di dalam lambda DiceRollerTheme{}, panggil fungsi DiceRollerApp().
MainActivity.kt


override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContent {
        DiceRollerTheme {
            DiceRollerApp()
        }
    }
}
Di fungsi DiceRollerApp(), panggil fungsi DiceWithButtonAndImage().
MainActivity.kt


@Preview
@Composable
fun DiceRollerApp() {
    DiceWithButtonAndImage()
}
Menambahkan pengubah
Compose menggunakan objek Modifier yang merupakan kumpulan elemen yang mendekorasi atau mengubah perilaku elemen UI Compose. Anda menggunakannya untuk menata gaya komponen UI dari komponen aplikasi Dice Roller.

Untuk menambahkan pengubah:

Ubah fungsi DiceWithButtonAndImage() untuk menerima argumen modifier dari jenis Modifier dan tetapkan nilai default Modifier.
MainActivity.kt


@Composable
fun DiceWithButtonAndImage(modifier: Modifier = Modifier) {
}
Cuplikan kode sebelumnya dapat membingungkan Anda. Jadi, mari kita perinci. Fungsi ini memungkinkan penerusan parameter modifier. Nilai default parameter modifier adalah objek Modifier sehingga bagian = Modifier dari tanda tangan metode. Nilai default parameter memungkinkan siapa saja yang memanggil metode ini di lain waktu yang akan memutuskan apakah akan meneruskan nilai untuk parameter atau tidak. Jika meneruskan objek Modifier-nya sendiri, pengguna dapat menyesuaikan perilaku dan dekorasi UI. Jika memilih untuk tidak meneruskan objek Modifier, objek tersebut akan dianggap sebagai nilai default, yaitu objek Modifier biasa. Anda dapat menerapkan praktik ini ke parameter apa pun. Untuk informasi selengkapnya argumen default, lihat Argumen default.

Catatan: Pernyataan import androidx.compose.ui.Modifier mengimpor paket androidx.compose.ui.Modifier, yang memungkinkan Anda mereferensikan objek Modifier.

Setelah composable DiceWithButtonAndImage() memiliki parameter pengubah, teruskan pengubah saat composable dipanggil. Tanda tangan metode untuk fungsi DiceWithButtonAndImage() mengalami perubahan sehingga objek Modifier dengan dekorasi yang diinginkan harus diteruskan saat dipanggil. Class Modifier bertanggung jawab atas dekorasi, atau penambahan perilaku, pada composable di fungsi DiceRollerApp(). Dalam hal ini, ada beberapa dekorasi penting yang dapat ditambahkan ke objek Modifier yang diteruskan ke fungsi DiceWithButtonAndImage().
Anda mungkin bertanya-tanya mengapa Anda harus bersusah payah meneruskan argumen Modifier ketika terdapat nilai default. Alasannya karena composable dapat menjalani rekomposisi yang pada dasarnya berarti blok kode dalam metode @Composable akan dijalankan lagi. Jika objek Modifier dibuat di blok kode, objek tersebut dapat berpotensi dibuat ulang dan tidak efisien. Rekomposisi dibahas nanti di codelab ini.

MainActivity.kt


DiceWithButtonAndImage(modifier = Modifier)
Buat rantai metode fillMaxSize() ke objek Modifier sehingga tata letak mengisi seluruh layar.
Metode ini menetapkan bahwa komponen harus mengisi ruang yang tersedia. Di bagian awal codelab ini, Anda telah melihat screenshot UI akhir aplikasi Dice Roller. Salah satu fitur pentingnya adalah dadu dan tombol berada di tengah layar. Metode wrapContentSize() menentukan bahwa ruang yang tersedia minimal harus sebesar komponen di dalamnya. Namun, karena metode fillMaxSize() digunakan, jika komponen di dalam tata letak lebih kecil dari ruang yang tersedia, objek Alignment dapat diteruskan ke metode wrapContentSize() yang menentukan cara komponen harus diratakan dalam ruang yang tersedia.

MainActivity.kt


DiceWithButtonAndImage(modifier = Modifier
    .fillMaxSize()
)
Catatan: Pernyataan impor untuk fillMaxSize() dan wrapContentSize() berturut-turut adalah import androidx.compose.foundation.layout.fillMaxSize dan import androidx.compose.foundation.layout.wrapContentSize.

Buat rantai metode wrapContentSize() ke objek Modifier, lalu teruskan Alignment.Center sebagai argumen untuk memusatkan komponen. Alignment.Center menentukan bahwa komponen dipusatkan secara vertikal dan horizontal.
MainActivity.kt


DiceWithButtonAndImage(modifier = Modifier
    .fillMaxSize()
    .wrapContentSize(Alignment.Center)
)
Catatan: Pernyataan impor untuk objek Alignment adalah import androidx.compose.ui.Alignment.

4. Membuat tata letak vertikal
Di Compose, tata letak vertikal dibuat dengan fungsi Column().

Fungsi Column() adalah tata letak composable yang menempatkan turunannya dalam urutan vertikal. Di desain aplikasi yang diharapkan, Anda dapat melihat bahwa gambar dadu ditampilkan secara vertikal di atas tombol lempar:

524ad07a9b61f729.png

Untuk membuat tata letak vertikal:

Dalam fungsi DiceWithButtonAndImage(), tambahkan fungsi Column().
Teruskan argumen modifier dari tanda tangan metode DiceWithImageAndButton() ke argumen pengubah Column().
Argumen modifier memastikan bahwa composable dalam fungsi Column() mematuhi batasan yang dipanggil pada instance modifier.

Teruskan argumen horizontalAlignment ke fungsi Column(), lalu tetapkan ke nilai Alignment.CenterHorizontally.
Ini memastikan bahwa turunan dalam kolom dipusatkan pada layar perangkat sesuai dengan lebarnya.

MainActivity.kt


fun DiceWithButtonAndImage(modifier: Modifier = Modifier) {
    Column (
        modifier = modifier,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {}
}

5. Menambahkan tombol
Di file strings.xml, tambahkan string dan tetapkan ke nilai Roll.
res/values/strings.xml


<string name="roll">Roll</string>
Dalam isi lambda Column(), tambahkan fungsi Button().
Catatan: Pernyataan impor untuk composable Button adalah import androidx.compose.material3.Button.

Di file MainActivity.kt, tambahkan fungsi Text() ke Button() dalam isi lambda fungsi.
Teruskan ID resource string dari string roll ke fungsi stringResource() dan teruskan hasilnya ke composable Text.
MainActivity.kt


Column(
    modifier = modifier,
    horizontalAlignment = Alignment.CenterHorizontally
) {
    Button(onClick = { /*TODO*/ }) {
        Text(stringResource(R.string.roll))
    }
}
Catatan: Jika Anda melengkapi fungsi Button() secara otomatis, argumen onClick = { /*TODO*/ } akan muncul. Jika tidak melengkapi otomatis atau Android Studio tidak mengizinkan, Anda dapat menerapkan argumen ini sendiri sebagai placeholder.

6. Menambahkan gambar
Komponen penting lain dari aplikasi ini adalah gambar dadu yang menampilkan hasil saat pengguna mengetuk tombol Lempar. Anda menambahkan gambar dengan composable Image, tetapi resource gambar juga diperlukan. Jadi, Anda harus mendownload beberapa gambar yang disediakan untuk aplikasi ini terlebih dahulu.

Mendownload gambar dadu
Buka URL ini untuk mendownload file zip gambar dadu ke komputer, lalu tunggu download selesai.
Temukan file di komputer Anda. File tersebut mungkin ada di folder Downloads.

Ekstrak file zip untuk membuat folder dice_images baru yang berisi enam file gambar dadu dengan nilai dadu dari 1 sampai 6.
Menambahkan gambar dadu ke aplikasi Anda
Di Android Studio, klik View > Tool Windows > Resource Manager.
Klik + > Import Drawables untuk membuka file browser.
Menu drop-down tambahkan resource di Resource Manager menampilkan opsi untuk mengimpor drawable.

Temukan dan pilih enam folder gambar dadu, lalu lanjutkan untuk menguploadnya.
Gambar yang diupload akan muncul sebagai berikut.

Jendela Import Drawables akan muncul dengan resource yang siap diimpor.

Klik Next.
Dialog konfirmasi impor akan muncul dan menampilkan lokasi file resource berada di struktur file. 

Dialog Import Drawables akan muncul dan menampilkan lokasi file resource berada di struktur file.

Klik Import untuk mengonfirmasi bahwa Anda ingin mengimpor enam gambar.
Gambar akan muncul di panel Resource Manager.

Panel Resource Manager menampilkan resource yang ada dalam project ini. 

Penting! Anda dapat melihat gambar tersebut dalam kode Kotlin dengan ID resource:

R.drawable.dice_1
R.drawable.dice_2
R.drawable.dice_3
R.drawable.dice_4
R.drawable.dice_5
R.drawable.dice_6
Bagus! Pada tugas berikutnya, Anda menggunakan gambar ini di aplikasi Anda.

Menambahkan composable Image
Gambar dadu akan muncul di atas tombol Roll. Compose menempatkan komponen UI secara berurutan. Dengan kata lain, composable mana pun yang dideklarasikan pertama kali akan ditampilkan terlebih dahulu. Artinya, deklarasi pertama ditampilkan di atas, atau sebelum, composable yang dideklarasikan setelahnya. Composable di dalam composable Column akan muncul di atas/di bawah composable lain di perangkat. Dalam aplikasi ini, Anda menggunakan Column untuk menumpuk Composable secara vertikal sehingga Composable mana pun yang dideklarasikan pertama dalam fungsi Column() akan ditampilkan sebelum composable dideklarasikan setelah itu dalam fungsi Column() yang sama.

Untuk menambahkan composable Image:

Pada isi fungsi Column(), buat fungsi Image() sebelum fungsi Button().
MainActivity.kt


Column(
    modifier = modifier,
    horizontalAlignment = Alignment.CenterHorizontally
) {
    Image()
    Button(onClick = { /*TODO*/ }) {
      Text(stringResource(R.string.roll))
    }
}
Catatan: Pernyataan impor untuk composable Image adalah import androidx.compose.foundation.Image.

Teruskan argumen painter ke fungsi Image(), lalu tetapkan nilai painterResource yang menerima argumen ID resource drawable. Untuk saat ini, teruskan ID resource berikut: argumen R.drawable.dice_1.
MainActivity.kt


Image(
    painter = painterResource(R.drawable.dice_1)
)
Catatan: Nantinya, Anda akan mengubah nilai yang diteruskan untuk ID resource. Untuk saat ini, kode harus diteruskan secara default sehingga kode akan dikompilasi untuk tujuan pratinjau.

Setiap kali membuat Gambar di aplikasi, Anda harus memberikan apa yang disebut dengan "deskripsi konten". Deskripsi konten adalah bagian penting dari pengembangan Android. Gambar menyediakan deskripsi ke komponen UI masing-masing untuk meningkatkan aksesibilitas. Untuk informasi deskripsi konten selengkapnya, lihat Menjelaskan setiap elemen UI. Anda dapat meneruskan deskripsi konten ke gambar sebagai parameter.
MainActivity.kt


Image(
    painter = painterResource(R.drawable.dice_1),
    contentDescription = "1"
)
Catatan: Deskripsi konten di atas adalah placeholder untuk saat ini. Ini akan diupdate di bagian selanjutnya dari codelab ini.

Kini semua komponen UI yang diperlukan sudah ada. Namun, Button dan Image sedikit saling bersinggungan.

92a1023933ee638a.png

Untuk memperbaikinya, tambahkan composable Spacer di antara composable Button dan Image. Spacer menggunakan Modifier sebagai parameter. Dalam hal ini, Image berada di atas Button sehingga harus ada spasi vertikal di antara keduanya. Oleh karena itu, tinggi Modifier dapat disetel agar berlaku untuk Spacer. Coba tetapkan tinggi ke 16.dp. Biasanya, dimensi dp diubah dengan kelipatan 4.dp.
MainActivity.kt


Spacer(modifier = Modifier.height(16.dp))
Catatan: Impor untuk composable Spacer, pengubah height, dan dp adalah:

import androidx.compose.foundation.layout.height

import androidx.compose.foundation.layout.Spacer

import androidx.compose.ui.unit.dp

Di panel Preview, klik Build & Refresh.
Anda akan melihat sesuatu yang mirip dengan gambar ini:

d893fc8fccb05813.png

7. Membuat logika pelemparan dadu
Setelah semua composable yang diperlukan tersedia, Anda akan memodifikasi aplikasi agar tombol melempar dadu jika diketuk.

Membuat tombol menjadi interaktif
Pada fungsi DiceWithButtonAndImage() sebelum fungsi Column(), buat variabel result dan tetapkan agar sama dengan nilai 1.
Lihat composable Button. Anda akan melihat parameter onClick yang ditetapkan ke sepasang tanda kurung kurawal dengan komentar /*TODO*/ di dalam kurung kurawal. Dalam hal ini kurung kurawal mewakili lambda, area di dalam kurung kurawal yang menjadi badan lambda. Jika diteruskan sebagai argumen, fungsi juga dapat disebut sebagai " callback".
MainActivity.kt


Button(onClick = { /*TODO*/ })
Lambda adalah literal fungsi yang merupakan fungsi seperti lainnya, tetapi bukannya dideklarasikan secara terpisah dengan kata kunci fun, lambda justru ditulis sebagai inline dan diteruskan sebagai ekspresi. Composable Buttonmenunggu fungsi diteruskan sebagai parameter onClick. Ini adalah tempat yang tepat untuk menggunakan lambda, dan Anda akan menulis isi lambda di bagian ini.

Dalam fungsi Button(), hapus komentar /*TODO*/ dari nilai isi lambda parameter onClick.
Lemparan dadu bersifat acak. Untuk menunjukkan hal itu dalam kode, Anda perlu menggunakan sintaksis yang benar untuk menghasilkan angka acak. Di Kotlin, Anda dapat menggunakan metode random() pada rentang nomor. Dalam isi lambda onClick, tetapkan variabel result ke rentang antara 1 hingga 6, lalu panggil metode random() pada rentang tersebut. Ingat bahwa dalam Kotlin, rentang ditentukan oleh dua titik antara angka pertama dalam rentang dan angka terakhir dalam rentang.
MainActivity.kt


fun DiceWithButtonAndImage(modifier: Modifier = Modifier) {
    var result = 1
    Column(
        modifier = modifier,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Image(painter = painterResource(imageResource), contentDescription = result.toString())
        Button(onClick = { result = (1..6).random() }) {
            Text(stringResource(R.string.roll))
        }
    }
}
Sekarang tombol tersebut dapat diketuk, tetapi ketukan pada tombol belum akan menyebabkan perubahan visual apa pun karena Anda masih harus membangun fungsi tersebut.

Menambahkan kondisional ke aplikasi dice roller
Di bagian sebelumnya, Anda telah membuat variabel result dan melakukan hard code pada nilai 1. Pada akhirnya, nilai variabel result akan direset saat tombol Roll diketuk dan akan menentukan gambar yang ditampilkan.

Composable bersifat stateless secara default, yang berarti bahwa composable tersebut tidak memiliki nilai dan dapat disusun ulang oleh sistem kapan saja sehingga mengakibatkan nilai tersebut direset. Namun, Compose menyediakan cara yang praktis untuk menghindari hal ini. Fungsi composable dapat menyimpan objek dalam memori menggunakan composable remember.

Ubah variabel result menjadi composable remember.
Composable remember memerlukan fungsi yang diteruskan.

Dalam isi composable remember, teruskan fungsi mutableStateOf(), lalu teruskan argumen 1 ke fungsi tersebut.
Fungsi mutableStateOf() akan menampilkan objek yang dapat diamati. Anda akan mempelajari observable lebih lanjut nanti. Namun, untuk saat ini, pada dasarnya ini berarti bahwa saat nilai variabel result berubah, rekomposisi akan dipicu, nilai hasilnya akan tercermin, dan UI akan di-refresh.

MainActivity.kt


var result by remember { mutableStateOf(1) }
Catatan: Pernyataan import androidx.compose.runtime.mutableStateOf dan import androidx.compose.runtime.remember mengimpor paket yang diperlukan untuk fungsi mutableStateOf() dan composable remember.

Pernyataan impor berikut juga diperlukan untuk mengimpor fungsi ekstensi yang diperlukan dari Status:

import androidx.compose.runtime.getValue

import androidx.compose.runtime.setValue

Sekarang, saat tombol diketuk, variabel result akan diupdate dengan nilai angka acak.

Sekarang, variabel result dapat digunakan untuk menentukan gambar yang akan ditampilkan.

Di bawah pembuatan instance variabel result, buat variabel imageResource tetap yang ditetapkan ke ekspresi when yang menerima variabel result, lalu tetapkan setiap kemungkinan hasil ke drawable-nya.
MainActivity.kt


val imageResource = when (result) {
    1 -> R.drawable.dice_1
    2 -> R.drawable.dice_2
    3 -> R.drawable.dice_3
    4 -> R.drawable.dice_4
    5 -> R.drawable.dice_5
    else -> R.drawable.dice_6
}
Ubah ID yang diteruskan ke parameter painterResource composable Image dari drawable R.drawable.dice_1 ke variabel imageResource.
Ubah parameter contentDescription composable Image untuk mencerminkan nilai variabel result dengan mengonversi variabel result menjadi string dengan toString() dan meneruskannya sebagai contentDescription.
MainActivity.kt


Image(painter = painterResource(id = imageResource), contentDescription = result.toString())
Jalankan aplikasi Anda.
Sekarang seharusnya aplikasi Dice Roller Anda sudah dapat berfungsi sepenuhnya.

524ad07a9b61f729.png

8. Mendapatkan kode solusi
Guna mendownload kode untuk codelab yang sudah selesai, Anda dapat menggunakan perintah git ini:


$ git clone https://github.com/Hendro10/Dice-Roller/edit/main.git
Atau, Anda dapat mendownload repositori sebagai file ZIP, lalu mengekstraknya, dan membukanya di Android Studio.

file_downloadDownload zip

Jika Anda ingin melihat kode solusi, lihat di GitHub.

Buka halaman repositori GitHub yang disediakan untuk project.
Pastikan nama cabang cocok dengan nama cabang yang ditentukan dalam codelab. Misalnya, dalam screenshot berikut, nama cabang adalah main (utama).
2301510b78db9764.png

Di halaman GitHub project, klik tombol Code yang akan menampilkan pop-up.
5844a1bc8ad88ce1.png

Pada pop-up, klik tombol Download ZIP untuk menyimpan project di komputer. Tunggu download selesai.
Temukan file di komputer Anda (mungkin di folder Downloads).
Klik dua kali pada file ZIP untuk mengekstraknya. Tindakan ini akan membuat folder baru yang berisi file project.
Membuka project di Android Studio
Mulai Android Studio.
Di jendela Welcome to Android Studio, klik Open.
4711318ba1db18a2.png

Catatan: Jika Android Studio sudah terbuka, pilih opsi menu File > Open.

e400aad673cc7e28.png

Di file browser, buka lokasi folder project yang telah diekstrak (kemungkinan ada di folder Downloads).
Klik dua kali pada folder project tersebut.
Tunggu Android Studio membuka project.
Klik tombol Run 1b472ca0dcd0297b.png untuk membangun dan menjalankan aplikasi. Pastikan aplikasi dibangun seperti yang diharapkan.

9. Kesimpulan
Anda telah membuat aplikasi Dice Roller interaktif untuk Android dengan Compose.

Ringkasan
Menentukan fungsi composable
Membuat tata letak dengan Komposisi.
Membuat tombol dengan composable Button.
Mengimpor resource drawable.
Menampilkan gambar dengan composable Image.
Membuat UI interaktif dengan composable.
Menggunakan composable remember untuk menyimpan objek dalam Komposisi ke memori.
Memuat ulang UI dengan fungsi mutableStateOf() agar dapat diamati.
Pelajari lebih lanjut
Tutorial Jetpack Compose
Menambahkan dependensi toolkit Jetpack Compose
Mulai menggunakan Jetpack Compose
Dasar-dasar Fungsi yang dapat dikomposisi
composable Button
composable Image
Status dan Jetpack Compose


