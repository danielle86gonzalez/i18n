# Akselerator

> Tentukan pintasan keyboard.

Accelerators adalah Strings yang dapat berisi banyak pengubah dan kode kunci, dikombinasikan dengan karakter ` + </ 0> , dan digunakan untuk menentukan cara pintas keyboard di seluruh aplikasi Anda.12eo</p>

<p spaces-before="0">Contoh:</p>

<ul>
<li><code>Command Or Control + A`</li>
* `Command Or Control + Shift + Z`</ul>

Jalan pintas terdaftar dengan modul
` globalShortcut </ 0> dengan menggunakan metode <a href="global-shortcut.md#globalshortcutregisteraccelerator-callback"><code> register </ 1> 
, misalnya</p>

<pre><code class="javascript">const { app, globalShortcut } = require('electron')

app.whenReady().then(() => {
  // Register a 'CommandOrControl+Y' shortcut listener.
  globalShortcut.register ('CommandOrControl + Y', () = & gt; {
     // Lakukan hal-hal saat Y dan salah satu Command / Control ditekan.
  })})
`</pre> 



## Pemberitahuan platform

Di Linux dan Windows , tombol ` Command </ 0> tidak berpengaruh sehingga gunakan <code> CommandOrControl </ 0> yang mewakili <code> Command </ 0> pada macOS dan <code> Control </ 0 > di Linux dan Windows untuk menentukan beberapa akselerator.</p>

<p spaces-before="0">Gunakan <code>Alt` dari pada `Option`. Tombol `Option` hanya tersedia di macOS, sedangkan tombol `Alt` tersedia di seluruh platform.

The ` super </ 0> kunci dipetakan ke <code> Windows </ 0> tombol pada Windows dan Linux dan
 <code> Cmd </ 0> di MacOS .</p>

<h2 spaces-before="0">Tersedia pengubah</h2>

<ul>
<li><code> Perintah </ 0> (atau <code> Cmd </ 0> sebentar)</li>
<li><code> Kontrol </ 0> (atau <code> Ctrl </ 0> sebentar)</li>
<li><code> CommandOrControl </ 0> (atau <code> CmdOrCtrl </ 0> untuk jangka pendek)</li>
<li><code>Alt`</li> 

* `Pilihan`
* `AltGr`
* `Bergeser`
* `Super`</ul> 



## Kode kunci yang tersedia

* ` 0 </ 0> sampai <code> 9 </ 0></li>
<li><code> A </ 0> ke <code> Z </ 0></li>
<li><code> F1 </ 0> sampai <code> F24 </ 0></li>
<li>Punctuation like <code>~`, `!`, `@`, `#`, `$`, etc.
* `Plus`
* `Ruang`
* `Tab`
* `Capslock`
* `Numlock`
* `Scrolllock`
* `Menghapus`
* `Menghapus`
* `Memasukkan`
* ` Kembali </ 0> (atau <code> Enter </ 0> sebagai alias)</li>
<li><code> Atas </ 0> , <code> Turun </ 0> , <code> Kiri </ 0> dan <code> Kanan </ 0></li>
<li><code> Beranda </ 0> dan <code> Akhir </ 0></li>
<li><code> Halaman Atas </ 0> dan <code> Halaman Bawah </ 0></li>
<li><code> Escape </ 0> (atau <code> Esc </ 0> singkatnya)</li>
<li><code> VolumeUp </ 0> , <code> VolumeDown </ 0> dan <code> VolumeMute </ 0></li>
<li><code> MediaNextTrack </ 0> , <code> MediaPreviousTrack </ 0> , <code> MediaStop </ 0> dan <code> MediaPlayPause </ 0></li>
<li><code>Layar cetak`
* Tombol Papan Angka 
    * `num0` - `num9`
  * `numdec` - tombol desimal
  * `numadd` - tombol angka `+`
  * `numsub` - tombol angka `-`
  * `nummult` - tombl angka`*`
  * `numdiv` - tombol angka `÷`