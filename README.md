# Reflection-seismic processing system (seisrefl) and apps — companion source for the book

Design documents and code for the reflection-seismic processing system built in ***Toward a Million Lines XXV — Building Reflection Seismic Processing Yourself, from Raw Data*** (Japanese title 『LLM で 100 万行のソフトウェア開発 XXV — 反射法地震探査の処理を生データから自分で作る』; the book is in Japanese).

The processing system `seisrefl` (Python) takes shot gathers, 2D or 3D, and produces stacked and prestack-migrated sections in time and in depth, and 3D volumes. It simulates raw data with a truth table, reads SEG-Y and HDF5 records, preprocesses them (trace editing, filters, deconvolution, datum and residual statics, first-break picking), analyses velocity (semblance, NMO picking, DMO) and migrates (Kirchhoff, phase-shift, PSPI, finite-difference and reverse-time migration, post-stack and prestack), and compares the results with the truth. Two **separate** Windows apps (Flutter) sit on top of it: a **simulator app** and a **processing app**. Both have Japanese and English screens.

## Download

**Ready-to-run apps and the source are on the [Releases](../../releases) page**, not in this repository. The release has three separate ZIPs:

| File | Size (bytes) | Contents |
|:---|---:|:---|
| `seisrefl_exe.zip` | 23,372,320 | The two Windows apps, ready built (**Japanese screens**), in separate folders (`processor\` and `simulator\`), with the source of the processing system (`tools_for_run\`) that both apps run |
| `seisrefl_exe_en.zip` | 23,373,037 | The same two apps (**English screens**) |
| `seisrefl.zip` | 2,210,784 | Design documents (`docs/`, Japanese, levels L1–L4: 360 files, the source of the code), the code (`src/seisrefl/`, 275 Python files; `app/`, the Flutter projects of the two apps and their shared part, and the launcher `.cmd` files), empty databases (`ledger/`, `product_db/`) and the license files |

## Requirements and how to start

- **Python 3.11 or later** with numpy, scipy, h5py and numba (`pip install .` installs them; see the README inside the ZIPs). Python is not bundled.
- **Windows** for the EXEs. There is **no APK**; the apps are Windows desktop apps. (The processing system itself is plain Python and also runs from a command prompt: `python -m seisrefl --help`.)
- Start in this order: **the simulator first** (press "make raw data"), then **the processing app** (pick that raw data as its input and press the stage buttons from the left). Without any data you can still run the whole chain this way. Each app starts from the `.cmd` file in its own folder.

## What is verified and what is not

- The two EXEs start on Windows 11 and show the window title in the chosen language. The processing system, on Python 3.11.9 (numpy 2.2.6, scipy 1.16.1, h5py 3.14.0, numba 0.61.2), runs the simulator sample settings in a few seconds on one thread.
- **The GPU path** (compute device `cuda`) has been checked only on Numba's CUDA simulator. **The speed on a real NVIDIA GPU has not been measured.** The default device is `cpu`.
- **Data are not included** (see License below); you fetch them yourself.
- **English design documents (`docs_en/`) and an English-default source tree (`src_en/`) are not included yet.** The English screens come from the same source switched with `--lang en`.
- The EXEs are **not code-signed**, so Windows may warn you. A warning is expected (tick "Unblock" in the ZIP's properties before extracting; if a warning appears, choose "More info" → "Run anyway").
- Tests and processing results are not included.

You are free to change the design documents and the code and compare the results.

---

## License

- **The author holds the copyright.**
- **The code is under the GNU General Public License v3.0 or later (GPL-3.0-or-later).** The full text is [`LICENSE`](LICENSE). **If you modify it and distribute it, you must publish the source under the same GPL-3.0-or-later terms.**
- **The design documents and the text are under the Creative Commons Attribution-ShareAlike 4.0 International license (CC BY-SA 4.0).** This covers `docs/`, the READMEs and the other written text of this repository, and the text of the matching book. See [`LICENSE-DOCS.md`](LICENSE-DOCS.md).
- **If you use them, say so.** State where it came from (the title of the book and the name of this repository) and keep the copyright notice. If you changed it, say that you changed it.
- **In this project the design documents are the source of the code.** When you publish something made from this code, **publish the design documents together with the code.**
- Third-party components (numpy, scipy, h5py, numba installed by `pip install .`; the Flutter engine inside the EXEs) remain under their own original licenses.
- The rights to the data used as examples in the book stay with the providers. **The data are not in this repository or in the ZIPs.** The book shows only our own processed figures and numbers.
  - **HardPicks, Brunswick 3D and Halfmile Lake 3D** (field data of 3D hard-rock reflection surveys): licensed under CC BY 4.0. Mila and Natural Resources Canada obtained licences from Glencore Canada Corporation (Brunswick) and Trevali Mining Corporation (Halfmile Lake) to distribute the field seismic data. Paper: P.-L. St-Charles et al., "A Multi-Survey Dataset and Benchmark for First Break Picking in Hard Rock Seismic Exploration", NeurIPS 2021 ML4PS workshop. The licence text was read on the project's data page and compared with the official page, the README of https://github.com/mila-iqia/hardpicks, on 2026-11-11; both state CC BY 4.0 for these data (the code of that repository is Apache-2.0, separate from the data licence).
  - **Poland 2D Vibroseis Line 001** (2D land vibroseis, prestack): from the SEG Wiki Open Data page "2D_Vibroseis_Line_001", which states that the data were donated to the public domain by Geofizyka Torun S.A., Poland.
- Provided "as is", without warranty — as the GPL-3.0 text says.

## 著作権とライセンス

- **著作権は、著者にあります。**
- **コードは、GNU General Public License v3.0 以降（GPL-3.0-or-later）です。**全文は [`LICENSE`](LICENSE) です。**改変して配布するときは、同じ GPL-3.0 以降の条件で、ソースを公開する義務があります。**
- **設計書と文章は、Creative Commons 表示-継承 4.0 国際（CC BY-SA 4.0）です。**このリポジトリの `docs/`・README ほかの文章と、対応する本の文章が当たります。内容は [`LICENSE-DOCS.md`](LICENSE-DOCS.md) です。
- **使ったときは、使ったことを書いてください。**どこから使ったか（本の題名と、このリポジトリの名前）と、著作権の表示を、使った先に書いてください。直したときは、直したことも書いてください。
- **このプロジェクトでは、設計書がコードの源です。**コードを使って作ったものを公開するときは、**設計書も、コードと一緒に公開してください。**
- 第三者の部品（`pip install .` で入る numpy・scipy・h5py・numba、EXE の中の Flutter の engine）は、それぞれの元のライセンスのままです。
- 本の例に使ったデータの権利は、提供元に残ります。**データはこのリポジトリにも ZIP にも含みません。**本に載せるのは私たちが処理した図と数だけです。
  - **HardPicks の Brunswick 3D と Halfmile Lake 3D**（硬岩域の 3D 反射法地震探査の実測の記録）: CC BY 4.0。Mila と Natural Resources Canada が、Glencore Canada Corporation（Brunswick）と Trevali Mining Corporation（Halfmile Lake）から、実測の記録を配る許諾を得ています。論文: P.-L. St-Charles ほか「A Multi-Survey Dataset and Benchmark for First Break Picking in Hard Rock Seismic Exploration」（NeurIPS 2021 ML4PS workshop）。ライセンスの文は、データの案内の頁で読み、2026-11-11 に公式の頁（`https://github.com/mila-iqia/hardpicks` の README）と照らしました。どちらも、これらのデータは CC BY 4.0 と書いています（その頁のコードは Apache-2.0 で、データのライセンスとは別です）。
  - **Poland 2D Vibroseis Line 001**（陸上の 2D バイブロサイスの重合前の記録）: SEG Wiki Open Data の頁「2D_Vibroseis_Line_001」によります。その頁に、Geofizyka Torun S.A.（ポーランド）がパブリックドメインに寄贈したと書かれています。
- 現状のまま（as is）提供します。保証はありません（GPL-3.0 の全文のとおりです）。
