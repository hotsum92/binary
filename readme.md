# binary

## create binary file

```sh
echo -en "\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f" > test.bin
ls -l test.bin | awk '{print $5}' # 16: print file size
```

echo -n: do not output trailing newline
echo -e: enable interpretation of backslash escapes
ls -l: list files with long format

## dump

```sh
echo -n {A..Z} | tr -d ' ' > test.bin
hexdump -C test.bin
xxd -g 1 test.bin
```

hexdump -C: display in hex and ASCII

## read n bytes

```sh
hexdump -n 8 test.bin # read 8 bytes
hexdump -n 8 -s 8 test.bin # read 8 bytes from offset 8
```

## display repeated bytes

```sh
echo -en "\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f" > test1.bin
echo -en "\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f" >> test1.bin
echo -en "\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f" >> test1.bin
echo -en "\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f" >> test1.bin
echo -en "\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f" >> test1.bin
hexdump -C test1.bin
hexdump -Cv test1.bin
```

## order

```sh
echo ABC | tr -d '\n' | iconv -f ASCII -t UCS-2 | xxd
echo ABC | tr -d '\n' | iconv -f ASCII -t UCS-2 | xxd -e
```

xxd -e: little-endian

## BOM

```sh
echo -en "\xFE\xFF" | xxd # big-endian
echo -en "\xFF\xFE" | xxd # little-endian
```

## sample

```sh
echo -en "\x00\x00\x00\x01" | \
od -tx4 -An -v --endian=big | \
tr ' ' '\n' | \
xargs -n1 -I{} printf '%d\n' 0x{}

od -tx4 -An -v --endian=big ./sample.dat | \
tr ' ' '\n' | \
xargs -n1 -I{} printf '%d\n' 0x{}
```
