# Useful links

## Scripts
---
- [GPIB でソースメータなどを制御するための script](https://github.com/yminononono/Quantum-GPIB)
- [PPMSデータ処理用の script](https://github.com/yminononono/Quantum-PPMS)
- [GDS design 作成用の script](https://github.com/yminononono/Quantum-GDSDesign)

## Software
---
- Design
	- [scQubits](https://scqubits.readthedocs.io/en/latest/guide/basics/basics.html)
	- [QuTip](https://qutip.org/try-qutip.html)
- Fit tools
	- [scikit-rf](https://scikit-rf.readthedocs.io/en/v1.6.2/index.html)
	- [resonator_tools](https://github.com/sebastianprobst/resonator_tools)
- Simulation
	- PyAEDT
	- qiskit-metal
	- pyEPR
	- [MPh](https://mph.readthedocs.io/en/1.2/index.html) : Pythonic scripting interface for Comsol Multiphysics
- GDS
	- klayout
	- PHIDL


## School
---
- [Quantum Sensing autumn school](https://indico.cern.ch/event/1421466/)
  - [Superconducting circuits in particle physics](https://indico.cern.ch/event/1421466/contributions/6012512/attachments/2963736/5213488/CERN_school_lecture.pdf)

## まとめサイト
---
- 電磁場解析のための電磁気学入門・電磁気学の基本的なことのあれこれ : https://www.photon-cae.co.jp/technicalinfo-list/
- 

## 資料
---
- [高周波回路入門](https://kobaweb.ei.st.gunma-u.ac.jp/lecture/2019-2-5matsuura.pdf)
- 量子コンピュータ
	- [超伝導量子コンピューターの実現に向けた技術開発](https://www.jstage.jst.go.jp/article/oubutsu/78/1/78_3/_pdf/-char/ja)
	- [超伝導量子コンピュータと希釈冷凍機](https://zenn.dev/qsrh/articles/quantum-computer-and-fridge-20241220#%E3%81%AA%E3%81%9C%E6%B8%9B%E8%A1%B0%E5%99%A8%E3%81%8C%E5%BF%85%E8%A6%81%E3%81%AA%E3%81%AE%E3%81%8B)
		- なぜ attenuator を使用するかや希釈冷凍機の原理について
		- ステージが金メッキされているのは無酸素銅の腐食を防ぐため
- 超伝導
	- [超伝導の基礎](http://accwww2.kek.jp/oho/textbook/from2022/2022/text/2_Arimoto.pdf)
	- [超伝導の基礎理論](https://www.jstage.jst.go.jp/article/oubutsu1932/71/11/71_11_1403/_pdf)

- Hardware
	-  SQUID
	    - [SQUIDの原理とシステム化技術](https://www.jstage.jst.go.jp/article/oubutsu1932/71/12/71_12_1534/_pdf)
	    - [The SQUID handbook](https://web.pa.msu.edu/people/edmunds/SQUID_Controller/References/sq_hb.pdf)
	-  HEMT
	    - https://www.fujitsu.com/jp/about/research/techguide/list/hemt/
	- JPA
		- [Superconducting Parametric Amplifiers](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9134828&tag=1)
- Fabrication
  - [洗浄技術のコツ](https://www.jstage.jst.go.jp/article/oubutsu/84/11/84_1009/_pdf/-char/ja)

## ブログ

- [量子ソフトウェア研究拠点テックブログ](https://zenn.dev/p/qsrh)

## Paper
- Qubit review
	- A Review of Design Concerns in Superconducting Quantum Circuits L https://arxiv.org/pdf/2411.16967v3
		- Simulation の違い (EPR, LOM, BBQ)

- Design (Qubit, Cavity, Feedline etc.)
	- [Coplanar waveguide resonators for circuit quantum electrodynamics](https://watermark02.silverchair.com/113904_1_online.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAABZ4wggWaBgkqhkiG9w0BBwagggWLMIIFhwIBADCCBYAGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMIv6zNZOBahP9Otn7AgEQgIIFUZyZQFpOKZ9wVYSgRlRkKke0njxr0vg5JdR81y4JSIzRVxpE213sw_7QP76uiYH69A3ha_8GB5jQuwoGSQNj-Ps2w334ulqXYfN2iEZTQfmVmXCqdpsbLQ73T3qev0ech5WZNIFTrct1wShqyvVr300wgL4hIGGLthsowbv6vY6KJdiCX6RiVIzENOgtJETWgAqwQ8xE1iKFFpV1CQNk-RKfFT7FuC_Ft3ma3t8hey7d4wF7U_F8TiJ6lKxO5sOp1v6Jy6vOa6V5VZu0Aw4McyMDwahmPFr2FfjiNnKEKopy1B_pJ6BgDS7a7ud-RM0QLg1NG-EVuV7NWxEPGHZk9-BbT1rtb0KZDBmuX_Mx4ra2vn5ciT3qcWsfTekM0NE84tf2P-mQYDCZKa-KDppb2a-oQY2M2MKFJZZ0ft33BZw11mIpru0UdeKRNu04NQJLmv8AnAyY1NYYARW5quwRuz3Jk__DlKDE113k8rmp5CjUlOV14uHNx4rGxvYKm0AJGWyScJWpVi7YZN4vlo-UuFetVMTU78VYw52XUg8e3ZaHDSDgNXPdKtpViHph-AB270IYrXuB4-8-O3bA7dtAUDj_eRNw3uY50o2kzGSMkh9voL5mUyNtOCvXagqx2PB3yw5BHfLqwH6txpz5zna_58Q2Q2h_G2KbfqjMK2yJG4tK0j3xuOf5w4TWwd3nkLGxWvJbysof_0HFoHthj_pRhT7erAdU7wtQR5buH8lWHf_7_NO99CfR3MB3IJAmcmYDNeW6kqSoh6u9HNSK6pSjBJSvvNDerVxbu2XPEf_ezwKYm2j7m_F3IbV5Al_as_fC1EK7ob58rW5zS1cxkm8CuCoA5mlyTFyN3Jb8oXNYRt2SMF8QNDa1tS0ZPxPaDnZRGAz7bi5n8i4jvm0vknL85pl2Hs6Us1kZuSWLnoM8M70Y2peqmwlkZya9XKvhnwiA4GldFzvMNfAB0CkJtLYhNCU4stgb0ip3gDhWeuOG6VMhGaVyBNU0t6tg6rf1puZq7jgirEm8nmGz3imqNq7PqHvcdweBcx-1Lc19HHFDHsn7GEnB9JBySM8JaSl3MVg14PUhels-o-d9Ya1Fk0HbXFu7EwzXngmoNhfSrFqgu7Ugk_ONl3LLmQdl_rZ3_uM-_tJisi8_7dC_-3sDvyLvneoqmsBXEEvyCGSwHrLYhLf7I2hzsvVZ76yyNvdQJitiBaOWmX_jRbbrCrh2HSdUYB69WcvVH-bNIEB0rK75_5yq1A3fvpCgeEPSjmGBoGxGc3JLPiOeVECIe5epMgx9fFMMErHhSxQVm5y74r2xYCfkSnANF_IF3gU-rZA7ReNRJzILfDkXLephG6y7l9Z8iMLK2I_iU0EE6jSat5Mz0jAZh7i0ffuluyyD1RPapwDujxzKcfiXTK-d3wewZ3dFYtuSV3SMfRAT_F4dhAL6APnYXE46CAId2khxVkdXfr_SXCvci68zArIgg0yAKw3SWaCxiJY_h5uLlJUhQOb7j3e0uW5W1ckkuAAON0gTSuQE5GoCWRtabzpJWGItAnI5yyGKEiSQ6swpOPE0_tmhGYJyQI59BjcB-hr65Z-QLSjc9QNwX7jWUtY1jY_r_Z_SinR9URzflFz0isGTWN2fmI0lil4P4IR-6uUM8F8E362cGtEAy-C8TT99yJW5kks7Whx1kya8buXLvxW_G1Yqyom4KdNAUP_P5fjjiJ9jGhmyEfx7VkKPk5LdJ99_KZmYxy0Bl5XO0mp7_XIsvO0e3TD5_cYbz7-Vyj1-xOyDFWxsEudRRkvkPGQHLI7kzTOka3M7)
		- Resonator と Feedline の coupling
	- [Experimentally verified, fast analytic and numerical design of superconducting resonators in flip-chip architectures](https://arxiv.org/pdf/2305.05502)
		- CPW の inductance や capacitance の計算

- Entangled states
	- [On the Speed-up of Wave-like Dark Matter Searches with Entangled Qubits](https://arxiv.org/pdf/2510.11795)
	- [Background Suppression in Quantum Sensing of Dark Matter via W State Projection](https://arxiv.org/pdf/2510.01816)
	- [Quantum Enhancement in Dark Matter Detection with Quantum Computation](https://link.aps.org/doi/10.1103/PhysRevLett.133.021801)
- Direct excitation
	- 
- Itinerant photon counting
	- Quantum non-demolition detection of an itinerant microwave photon : https://www.nature.com/articles/s41567-018-0066-3
- Galvanic coupling
	- 3D implementation : https://iopscience.iop.org/article/10.1088/1367-2630/18/10/103036/pdf
- Schuster labのhigh freq qubitの論文
    - Nb trilayer transmonのレシピ: https://arxiv.org/pdf/2306.05883
    - それで24GHzのqubit作った: https://arxiv.org/abs/2402.03031
    - さらに72GHz qubitの作った: https://arxiv.org/abs/2411.11170
- Single photon detection
	- Cavity haloscope + Single photon detection (Dixit etal.)
		- [Searching for Dark Matter with a Superconducting Qubit](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.126.141302)
	- Cavity haloscope + Single photon detection + SQUID (Dixit etal.)
		- [A Flux-Tunable Cavity for Dark Matter Detection](https://arxiv.org/pdf/2501.06882)
	- Fock state を使った dark matter detection (Aaron chou etal.)
		- [Stimulated Emission of Signal Photons from Dark Matter Waves](https://journals.aps.org/prl/pdf/10.1103/PhysRevLett.132.140801)
- Seamless cavities
	- Coax cavity
		- [Erasure detection of a dual-rail qubit encoded in a double-post superconducting cavity](https://arxiv.org/pdf/2311.04423)
		- https://arxiv.org/pdf/2006.02213
	- Flute cavity
		- https://journals.aps.org/prl/pdf/10.1103/PhysRevLett.127.107701
- Axion Searches
	- BREAD 
		- [First Axionlike Particle Results from a Broadband Search for Wavelike Dark Matter in the 44 to 52 μeV Range with a Coaxial Dish Antenna](https://journals.aps.org/prl/pdf/10.1103/PhysRevLett.134.171002)
	- ORGAN
		- https://indico.global/event/647/contributions/16622/attachments/5344/8618/McAllister_UCLA_DM.pdf
	- RADES
		- [RADES axion search results with a High-Temperature Superconducting cavity in an 11.7 T magnet](https://arxiv.org/pdf/2403.07790)
	- 

## Specific topics

- Q値について
	- [Efficient methods for extracting superconducting resonator loss in the single-photon regime](https://pubs.aip.org/aip/jap/article/137/4/044401/3332568/Efficient-methods-for-extracting-superconducting)
		- Circle fit でやっていることを step-by-step で説明
	- [Efficient and robust analysis of complex scattering data under noise in microwave resonators](https://pubs.aip.org/aip/rsi/article/86/2/024706/360955/Efficient-and-robust-analysis-of-complex)
	- [Measurement of resonant frequency and quality factor of microwave resonators: Comparison of methods](https://pubs.aip.org/aip/jap/article/84/6/3392/488543/Measurement-of-resonant-frequency-and-quality)
		- 3dB 法や circle fit など Q-factor の求め方の比較
- Fano interference (ファノ共鳴)
	- [Fano Interference in Microwave Resonator Measurements](https://journals.aps.org/prapplied/pdf/10.1103/PhysRevApplied.20.014059)
- Gap engineering
	- [Resisting High-Energy Impact Events through Gap Engineering in Superconducting Qubit Arrays](https://journals.aps.org/prl/pdf/10.1103/PhysRevLett.133.240601)
	- [Recovery Dynamics of a Gap-Engineered Transmon after a Quasiparticle Burst](https://journals.aps.org/prl/pdf/10.1103/ql6q-wfpn)
	- [Exploring the Effect of Radioactive Sources on Gap-Engineered Superconducting Qubits](https://indico.global/event/14966/contributions/133994/attachments/63163/121926/pinckney_gap_eng_CPAD_2025.pdf) ← スライド