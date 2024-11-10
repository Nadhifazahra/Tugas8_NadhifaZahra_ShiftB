# Proses CRUD

## Tampilan Data Mahasiswa </br>
<img src="https://github.com/user-attachments/assets/2bf1dceb-52fc-4afb-9b70-1ef2686337c2" width="400"> </br>
Awal membuka aplikasi langsung menampilkan halaman data mahasiswa. Di dalamnya terdapat data mahasiswa dan button tambah mahasiswa. Tampilan tambah data mahasiswa didapat dari kode berikut
``` html
<ion-header [translucent]="true">
  <ion-toolbar>
    <ion-title>Data Mahasiswa</ion-title>
  </ion-toolbar>
</ion-header>

<ion-content [fullscreen]="true">
  <ion-header collapse="condense">
    <ion-toolbar>
      <ion-title size="large">Data Mahasiswa</ion-title>
    </ion-toolbar>
  </ion-header>

  <ion-card *ngFor="let item of dataMahasiswa">
    <ion-item>
      <ion-label>
        {{item.nama}}
        <p>{{item.jurusan}}</p>
      </ion-label>
      <ion-button color="danger" slot="end" (click)="confirmHapusMahasiswa(item.id)"
        >Hapus</ion-button
      >
      <ion-button expand="block" (click)="openModalEdit(true,item.id)"
        >Edit</ion-button
      >
    </ion-item>
  </ion-card>
  <!-- button tambah -->
  <ion-card>
    <ion-button (click)="openModalTambah(true)" expand="block"
      >Tambah Mahasiswa</ion-button
    >
  </ion-card>
```

## Tambah Mahasiswa
### a. Form Tambah Mahasiswa </br>
<img src="https://github.com/user-attachments/assets/34cb0955-7118-4f6b-a8f8-b7329d7c520c" width="400">
<img src="https://github.com/user-attachments/assets/d76deb4a-d216-4c5e-bd9f-ef072d6d9d0a" width="400"> </br>
Jika button tambah mahasiwa pada halaman data mahasiswa di klil, maka akan menampilkan halaman form tambah mahasiswa. Form ini berisi nama dan jurusan yang dapat diisi. Terdapat juga button batal yang jika di klik akan mengembalikan halaman ke halaman data mahasiswa. Button tambah mahasiswa pada halaman ini akan menyimpan hasil input form ke dalam database. Tampilan form tambah mahasiswa didapat dari kode berikut

```html
<ion-modal [isOpen]="modalTambah">
    <ng-template>
      <ion-header>
        <ion-toolbar>
          <ion-buttons slot="start">
            <ion-button (click)="cancel()">Batal</ion-button>
          </ion-buttons>
          <ion-title>Tambah Mahasiswa</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <ion-item>
          <ion-input
            label="Nama Mahasiswa"
            labelPlacement="floating"
            required
            [(ngModel)]="nama"
            placeholder="Masukkan Nama Mahasiswa"
            type="text"
          >
          </ion-input>
        </ion-item>

        <ion-item>
          <ion-input
            label="Jurusan Mahasiswa"
            labelPlacement="floating"
            required
            [(ngModel)]="jurusan"
            placeholder="Masukkan Jurusan Mahasiswa"
            type="text"
          >
          </ion-input>
        </ion-item>
        <ion-row>
          <ion-col>
            <ion-button
              type="button"
              (click)="tambahMahasiswa()"
              color="primary"
              shape="full"
              expand="block"
              >Tambah Mahasiswa
            </ion-button>
          </ion-col>
        </ion-row>
      </ion-content>
    </ng-template>
  </ion-modal>
```

### b. Hasil Tambah Mahasiswa </br>
<img src="https://github.com/user-attachments/assets/1117b20b-80ce-481a-9a9a-6c689bae8e53" width="400"> </br>
Ketika button tambah mahasiswa pada halaman form tambah mahasiswa di klik, maka akan mengaktifkan fungsi tambahMahasiswa() di bawah. Hasil input form akan tampil di halaman data mahasiswa beserta button edit dan delete.

``` html
  tambahMahasiswa() {
    if (this.nama != '' && this.jurusan != '') {
      let data = {
        nama: this.nama,
        jurusan: this.jurusan,
      };
      this.api.tambah(data, 'tambah.php').subscribe({
        next: (hasil: any) => {
          this.resetModal();
          console.log('berhasil tambah mahasiswa');
          this.getMahasiswa();
          this.modalTambah = false;
          this.modal.dismiss();
        },
        error: (err: any) => {
          console.log('gagal tambah mahasiswa');
        },
      });
    } else {
      console.log('gagal tambah mahasiswa karena masih ada data yg kosong');
    }
  }
  ```

## Edit Mahasiswa
### a. Form Edit Mahasiswa </br>
<img src="https://github.com/user-attachments/assets/84104917-2f96-4fe0-bb4b-88398d3dde16" width="400">
<img src="https://github.com/user-attachments/assets/42a71ea6-7ec9-4902-9a9d-e6fd7fbaa957" width="400"> </br>
Ketika button edit pada halaman data mahasiswa di klik, maka akan menampilkan halaman form edit mahasiswa. Halaman ini berisi form yang telah terisi dan button edit mahasiswa. Tampilan ini didapatkan dari kode berikut

``` html
 <ion-modal [isOpen]="modalEdit">
    <ng-template>
      <ion-header>
        <ion-toolbar>
          <ion-buttons slot="start">
            <ion-button (click)="cancel()">Batal</ion-button>
          </ion-buttons>
          <ion-title>Edit Mahasiswa</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-content class="ion-padding">
        <ion-item>
          <ion-input
            label="Nama Mahasiswa"
            labelPlacement="floating"
            required
            [(ngModel)]="nama"
            placeholder="Masukkan Nama Mahasiswa"
            type="text"
          >
          </ion-input>
        </ion-item>
        <ion-item>
          <ion-input
            label="Jurusan Mahasiswa"
            labelPlacement="floating"
            required
            [(ngModel)]="jurusan"
            placeholder="Masukkan Jurusan Mahasiswa"
            type="text"
          >
          </ion-input>
        </ion-item>
        <ion-input required [(ngModel)]="id" type="hidden"> </ion-input>
        <ion-row>
          <ion-col>
            <ion-button
              type="button"
              (click)="editMahasiswa()"
              color="primary"
              shape="full"
              expand="block"
              >Edit Mahasiswa
            </ion-button>
          </ion-col>
        </ion-row>
      </ion-content>
    </ng-template>
  </ion-modal>
</ion-content>
```


### b. Hasil Edit Mahasiswa </br>
<img src="https://github.com/user-attachments/assets/7f2d3723-5d4b-48d3-92ca-7c937a4da19f" width="400"></br>
Jika button edit mahasiswa pada halaman form edit mahasiswa di klik, maka akan mengaktifkan fungsi editMahasiswa() seperti di bawah. Fungsi ini akan menyimpan perubahan ke database
``` html
editMahasiswa() {
    let data = {
      id: this.id,
      nama: this.nama,
      jurusan: this.jurusan,
    };
    this.api.edit(data, 'edit.php').subscribe({
      next: (hasil: any) => {
        console.log(hasil);
        this.resetModal();
        this.getMahasiswa();
        console.log('berhasil edit Mahasiswa');
        this.modalEdit = false;
        this.modal.dismiss();
      },
      error: (err: any) => {
        console.log('gagal edit Mahasiswa');
      },
    });
  }
```

## Hapus Mahasiswa
### a. Alert Confirm Hapus </br>
<img src="https://github.com/user-attachments/assets/1e4cd057-27a6-4538-a66c-8546b7d44da0" width="400"> </br>
Ketika button delete di data mahasiswa di klik, maka akan menampilkan alert confirm hapus yang didapat dari fungsi berikut
```
async confirmHapusMahasiswa(id: any) {
    const alert = await this.alertController.create({
      header: 'Konfirmasi Hapus',
      message: 'Apakah Data ingin dihapus?',
      buttons: [
        {
          text: 'Tidak',
          role: 'cancel',
          handler: () => {
            console.log('Penghapusan dibatalkan');
          },
        },
        {
          text: 'Ya',
          handler: () => {
            this.hapusMahasiswa(id); // Panggil fungsi hapusMahasiswa jika pengguna memilih "Ya"
          },
        },
      ],
    });

    await alert.present();
  }
```

### b. Hasil Hapus Mahasiswa </br> 
<img src="https://github.com/user-attachments/assets/c1269247-a0db-4c05-b0fa-f1915ebb7f0e" width="400"> </br>
Jika button 'Ya' pada alert confirm hapus di klik, maka akan mengaktifkan fungsi di bawah ini dan data terhapus. Jika button 'Tidak' di klik maka akan kembali ke halaman data mahasiswa.
```
 hapusMahasiswa(id: any) {
    this.api.hapus(id, 'hapus.php?id=').subscribe({
      next: (res: any) => {
        console.log('sukses', res);
        this.getMahasiswa();
        console.log('berhasil hapus data');
      },
      error: (error: any) => {
        console.log('gagal');
      },
    });
  }
```




