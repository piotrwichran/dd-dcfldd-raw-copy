# dd-dcfldd-raw-copy

```bash
sudo dd if=/dev/sdb of=/mnt/evidence/dysk.img bs=4M conv=noerror,sync status=progress
```
- if= → input file (źródło)
- of= → output file (obraz)
- bs=4M → szybsze kopiowanie
- conv=noerror,sync → nie przerywa przy błędach i zachowuje offset
- status=progress → pokazuje postęp
  
```bash
sudo dcfldd if=/dev/sdb of=/mnt/evidence/dysk.img hash=sha256 hashlog=hash.txt bs=4M conv=noerror,sync
```

## Typ nośnika

SSD / NVMe:
- zwykle większe bs (4–16M)
- szybki kontroler

HDD:
- optymalnie 1–4M
- zbyt duże bs może spowolnić przy błędach.

## Interfejs
- USB 2.0 → mniejsze bs stabilniejsze
- USB 3.x / SATA / NVMe → większe bs OK.

Błędy na dysku (forensic realia)
Przy uszkodzonym dysku:
👉 duże bs = większy fragment problematyczny.
Dlatego często:
```code
bs=512K lub 1M
conv=noerror,sync.
```
## Duże obrazy (500 GB – kilka TB)
```bash
sudo dd if=/dev/sdb of=/mnt/evidence/dysk.img \
bs=4M conv=noerror,sync oflag=direct status=progress
```
## Uwaga techniczna

oflag=direct wymaga:
- aby bs był wielokrotnością rozmiaru bloku systemu plików (zwykle 512 B lub 4K).

Dlatego:
👉 bs=4M działa dobrze,
👉 bs=1000 może się wywalić.
