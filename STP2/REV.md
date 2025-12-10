ran file on the binary 
<img width="1761" height="60" alt="Screenshot_20251210_121641" src="https://github.com/user-attachments/assets/4c7f7354-412a-4a78-a24a-0e946a10b2af" />
changed the permission to execute and then i executed but it needed an input with it 
if u give input less than 16 , i says input must be of size 16 , and if i gave an input of exact 16 and it goes into this loop
<img width="676" height="127" alt="Screenshot_20251210_122018" src="https://github.com/user-attachments/assets/9fadcbee-8216-40ce-8761-e1f41fde1fa0" />
opned the file in ghidra 
```
undefined8 FUN_00101d44(int param_1,undefined8 *param_2)

{
  undefined4 uVar1;
  size_t sVar2;
  void *__src;
  void *__dest;
  undefined8 uVar3;
  long lVar4;
  code *pcVar5;
  ulong local_10;
  
  if (param_1 < 2) {
    fprintf(stderr,"usage: %s input\n",*param_2);
                    /* WARNING: Subroutine does not return */
    exit(1);
  }
  sVar2 = strlen((char *)param_2[1]);
  if (sVar2 != 0x10) {
    fwrite("input must be of size 16",1,0x18,stderr);
                    /* WARNING: Subroutine does not return */
    exit(1);
  }
  DAT_0010a310 = calloc(8,0x20000);
  FUN_00101238(0xadc52d,FUN_001014cd);
  FUN_00101238(0x560729d,FUN_00101652);
  FUN_00101238(0x48c5ccc6,FUN_00101438);
  FUN_00101238(0x542010a0,FUN_00101590);
  FUN_00101238(0xbdecfe55,FUN_001017bb);
  FUN_00101238(0x41f93b4b,FUN_001017d4);
  FUN_00101238(0x5e64bb6c,FUN_001017ea);
  FUN_00101238(0xed4e2cfb,FUN_00101835);
  FUN_00101238(0x180bc12d,FUN_00101866);
  FUN_00101238(0x5a0f38fc,FUN_001018f9);
  FUN_00101238(0x27497906,FUN_0010198c);
  FUN_00101238(0xba1116a9,FUN_001019a2);
  FUN_00101238(0xfa83fa5e,FUN_001019de);
  FUN_00101238(0x818cd6b5,FUN_00101a1d);
  FUN_00101238(0x8d67bae1,FUN_00101a64);
  FUN_00101238(0xd1450d67,FUN_00101abf);
  FUN_00101238(0x8ea45b38,FUN_00101b46);
  FUN_00101238(0xf00bb6c1,FUN_00101b03);
  FUN_00101238(0x5991ba22,FUN_00101b89);
  FUN_00101238(0x43ae1f53,FUN_00101724);
  FUN_00101238(0x8960888a,FUN_00101be5);
  FUN_00101238(0x1f0a8e6f,FUN_00101c41);
  FUN_00101238(0x466a54d9,FUN_00101c8e);
  FUN_00101238(0xfb521a9c,FUN_00101ca4);
  FUN_00101238(0xc650f15d,FUN_00101cf7);
  __src = (void *)FUN_001020c6(&DAT_001050a0,0x3f10);
  sVar2 = strlen((char *)param_2[1]);
  __dest = calloc(1,sVar2 + 0x2f4c);
  uVar3 = FUN_001020c6(&DAT_00108fc0,0x1328);
  memcpy(__dest,__src,0x2f4c);
  sVar2 = strlen((char *)param_2[1]);
  memcpy((void *)((long)__dest + 0x2f4c),(void *)param_2[1],sVar2);
  lVar4 = FUN_001012bc(uVar3,__dest);
  puts("[fr] Ne quittez pas, un correspondant va prendre votre appel... [\\fr]");
  local_10 = 0;
  while (*(char *)(lVar4 + 0x80) != '\0') {
    uVar1 = FUN_0010137f(lVar4);
    pcVar5 = (code *)FUN_001011d5(uVar1);
    (*pcVar5)(lVar4);
    local_10 = local_10 + 1;
    if (((local_10 & 0xfffffff) == 0) && (*(char *)(lVar4 + 0x88) != '\x01')) {
      puts("[fr] Ne quittez pas, un correspondant va prendre votre appel... [\\fr]");
    }
  }
  return 0;
}

```
found the function that was giving the output 
searche what the function did and it looked like the funtion is a vm where the execution is done my many different funtions, 
