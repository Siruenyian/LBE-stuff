# LBE-stuff

## Contents
1. [Unity-Installation](#unity-installing-just-in-case--launch-unity-hub)
2. [Project-Setup](#making-the-project-2d)
3. [Scene-Creation](#first-scene)
4. [Unity-Objects](#objects-components-sprite-renderers-already-in-the-previous-section)
5. [Player-Movement](#making-the-players-script)
6. [Player-Movement testing](#test-the-player-script-in-this-case-involves-the-players-movement)
7. [Tilemap-Setup](#configuring-tilemap)
8. [Tilemap-Drawing](#making-the-map-using-the-walls-from-tilemap)
9. [Map-Collision](#wallcoll-scripts--finish-line-triggers)
10. [Finishing-Build](#build-to-desired-platform-pc-for-this-game)
10. [ඞ](#amogus-sussy-baka)

## Unity Installing (just in case) + launch unity hub

Link : https://unity.com/download

Klik pilihan sesuai OS yang kalian gunakan.
![image](https://user-images.githubusercontent.com/80830860/194204319-a9e47691-bd1f-494e-8814-3121bf3740e1.png)

Setelah selesai install, buka Unity Hub.

![image](https://user-images.githubusercontent.com/80830860/194204458-1613e29c-5aff-42b0-ab5b-7e68a81424e6.png)

## making the project (2d)

Setelah Unity Hub terbuka, klik "New project"
![image](https://user-images.githubusercontent.com/80830860/194204509-b72f0ccf-1ae4-475f-8542-318de630edab.png)

Klik template apa yang ingin digunakan untuk project. Untuk project kali ini, pilihlah "2D".
![image](https://user-images.githubusercontent.com/80830860/194204707-3d107c8f-0119-4859-8ecf-d84b10556247.png)
Beri nama project sesuai yang diinginkan beserta directory-nya. Sudah semua? Klik "Create Project"!

## first scene

Selamat datang di project-mu! Silakan lihat-lihat sekitar terlebih dahulu~
![image](https://user-images.githubusercontent.com/80830860/194207690-abfdf661-2837-4284-8033-bba2cae02556.png)

Sudah? Yuk, kita buat scene pertama yang akan kita gunakan di project kali ini~

![image](https://user-images.githubusercontent.com/80830860/194207928-55ec2d05-bbd7-4da1-b316-e5b2d49b72d5.png)

Wah, ternyata Unity sudah membuatkan kita scene untuk langsung dipakai. Namun, gimana kalau misal kita mau menggunakan scene lain nantinya?

Klik kanan di window Project ~> Create ~> Scene. Selamat, scene baru telah terbuat! (Disarankan untuk membuat scene di folder Scenes yang terdapat didalam folder Assets agar file lebih rapi!)

![image](https://user-images.githubusercontent.com/80830860/194208096-57afad1e-e56a-4c4a-a50a-92937b3665ba.png)

![image](https://user-images.githubusercontent.com/80830860/194208462-576253f7-0810-4c5e-9ada-d92fb5e97627.png)

## making an object (leads to player)

Sekarang, mari kita buat suatu object yang akan menjadi player kita nantinya.
Di tab Hierarchy, klik kanan ~> Create Empty. Pilihan tersebut akan membuat suatu objek (dengan default name GameObject) yang tidak berisi apa-apa.

![image](https://user-images.githubusercontent.com/80830860/194211227-5d75139a-f40c-4ea4-8f68-a16b8af6aa6b.png)

![image](https://user-images.githubusercontent.com/80830860/194211605-5f81ca87-ed6e-4e67-af43-5d47ead0532e.png)

Waduh, objeknya terlihat transparan di tab Scene maupun Game, ya? Tidak apa-apa, ayo kita beri sesuatu di objek tersebut agar dapat terlihat di kedua tab tersebut!

Klik GameObject dari tab Hierarchy. Tab Inspector akan memberi tahu kita komponen apa saja yang telah terikat pada object tersebut.

Kemudian klik "Add Component" dan ketik "Sprite Renderer" pada tempat yang telah disediakan, dan klik Sprite Renderer.

![image](https://user-images.githubusercontent.com/80830860/194212154-48577817-7740-483b-bb7b-f5a7e5f023f7.png)

Komponen Sprite Renderer tersebut bekerja sebagaimana yang telah tertulis di namanya, untuk me-render sprite yang tersedia.

Sekarang, kembali ke tab Assets dan buatlah folder bernama "Sprites". Kemudian, masukkan suatu gambar yang ingin kalian gunakan sebagai sprite dari objek yang akan kita gunakan nantinya ke folder tersebut.

![image](https://user-images.githubusercontent.com/80830860/194212488-7b7f9115-642c-46fa-a73c-6161ee3503a7.png)

Nah, bagaimana caranya agar objek tersebut menggunakan gambar yang telah kita masukkan tadi?

Klik dan tahan gambar tersebut dan drag ke tempat "Sprite" di komponen Sprite Renderer dari objek.

![image](https://user-images.githubusercontent.com/80830860/194212757-65297856-01f5-4c8b-bd12-d33f2ce6b3a8.png)

Hore! Objek tersebut sekarang memiliki gambar yang dapat terlihat di tab Scene dan Game~

## object's components (sprite renderer's already in the previous section)

here - dont forget player scale 0.4 and player tag

## making the player's script 

Saatnya coding 💀

Bukalah tab Assets dan buatlah folder bernama "Scripts". Dalam folder scripts, buatlah folder lagi yang bernama "Movement".
Didalam folder Movement klik kanan ~> Create ~> C# Script dan namakan script PlayerMovementScript.cs.
![image](https://user-images.githubusercontent.com/92420947/194622251-7334b46e-71ab-4c41-bcc2-ca4b4ccb4f4c.png)

Kemudian, klik dua kali script tersebut untuk membukanya pada Visual Studio.
Setelah terbuka, isikan script dengan kode berikut

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMovementScript : MonoBehaviour
{

    private Rigidbody2D rb;
    private SpriteRenderer sprite;
    private Vector2 inputDirection;
    [SerializeField]private float speed=3.0f;
    

    // Start is called before the first frame update
    void Start()
    {
        //Get component from the objects attached
        rb = GetComponent<Rigidbody2D>();
        sprite = GetComponent<SpriteRenderer>();
    }

    // Update is called once per frame
    void Update()
    {
        FlipSpite();
    }

    private void FixedUpdate()
    {
        Move();
    }

    private void Move()
    {   
        //DeclAre variable for capturing input(unity eventsystem)
        float horizontal = Input.GetAxisRaw("Horizontal");
        float vertical = Input.GetAxisRaw("Vertical");

        //Normalize input directions
        inputDirection = new Vector2(horizontal, vertical).normalized;
        //Set velocity
        rb.velocity = inputDirection * speed;
    }

    void FlipSpite()
    {
        
        Debug.Log(inputDirection.x+" "+sprite.flipX);
        if (inputDirection.x < 0)
        {
            Debug.Log("idk1");
            sprite.flipX = false;
        }
        else if (inputDirection.x > 0)
        {
            Debug.Log("idk2");
            sprite.flipX = true;
        }
    }

}

```

## test the player script (in this case involves the player's movement)

Untuk mencoba player script yang telah dibuat, klik GameObject player, kemudian klik "Add Component" dan ketik "PlayerMovementScript.cs" pada tempat yang telah disediakan, dan klik scriptnya.
Selamat, script telah ditambahkan dan sekarang player dapat bergerak sesuai input keyboard!

![image](https://im2.ezgif.com/tmp/ezgif-2-835e293293.gif)

## configuring tilemap

Nah, beralih ke bagian tilemap. Pada tab Hierarchy, klik kanan, select 2D Object ~> Tilemap ~> Rectangular

![image](https://user-images.githubusercontent.com/92420947/194599776-8083733e-a69f-40cd-82c4-a953f59216e2.png)

Sekarang muncul gameobject dan gridline pada scene

![image](https://user-images.githubusercontent.com/92420947/194601411-70335c6b-713c-4d7a-a21b-a9564bbe7773.png)

Selanjutnya, tilemap sudah dibuat tapi masih belum memiliki isi. Untuk mengisi tilemap, diperlukan sebuah brush yang diakses lewat Tile Palete.

Untuk membuka Tile Palette, pada menu paling atas(File, etc. ) klik Window ~> 2D ~> Tile Palette

![image](https://user-images.githubusercontent.com/92420947/194599551-94f5586a-63c2-4014-9341-c603f3f78618.png)

Setelah tile palette terbuka, klik Create New Palette dan beri nama palette yang akan dibuat. Lalu klik Create dan save Palette pada folder baru bernama Tilemaps

![image](https://user-images.githubusercontent.com/92420947/194603837-4acf9885-cf44-4e6b-9f41-09cf7a62e2b2.png)

Akhirnya sampai kepada langkah untuk membuat brush. Pada panel Project, cari asset yang bernama square. Hasil search yang tampil merupakan sprite bawaan unity yang akan digunakan untuk kebutuhan tilemap.

![image](https://user-images.githubusercontent.com/92420947/194605583-44504678-4424-46e7-ba1d-d54f602e452a.png)

Selanjutnya, drag and drop image square kedalam panel Tile Palette, save asset pada folder Tilemaps. 

Pastikan sprite square memiliki tipe collider yang tepat dengan menseleksi spritenya pada panel Projects, membuka inspector, dan mengubah collider type menjadi sprite
![image](https://user-images.githubusercontent.com/92420947/194613241-540a4016-af0b-4d6a-9202-63114b82e498.png)


YAY! Tilemap berhasil dibuat!

## making the map using the walls from tilemap

Saatnya menggambar tilemap!

Tunggu dulu! Sebelum menggambar, pastikan brush yang benar sudah terpilih.
Pada panel Tile Palette klik "Paint with active brush" (shortcut key B) untuk memilih brush yang digunakan untuk menggambar tilemap. 

![image](https://user-images.githubusercontent.com/92420947/194611455-3472d6b3-804a-48cb-9b10-485b01fbb297.png)

Jangan lupa untuk memilih tile box yang sudah ada pada Tile Palette dengan cara melakukan klik kiri. 

Setelah brush terpilih, saatnya menggambar pada scene!
Buka kembali panel Scene, dengan brush yang sudah terpilih sprite box dapat ditempatkan dimanapun dalam grid layaknya menggunakan kuas.

![image](https://user-images.githubusercontent.com/92420947/194616094-78202584-e5de-4096-9725-d904edbc9b5c.png)

Penggambaran tile inilah yang akan digunakan untuk membangun map maze.

Berikut penggambaran map yang akan digunakan untuk bagian selanjutnya tutorial ini
![image](https://user-images.githubusercontent.com/92420947/194616768-b61af141-f284-46f1-a6b9-7e2c581794a9.png)

## wallcoll scripts + finish line (triggers)

here

## build to desired platform (pc for this game)

here

# amogus sussy baka
