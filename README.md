# notation-cose

A *minimum viable prototype* of notation plugin for [COSE](https://datatracker.ietf.org/doc/html/rfc8152) signatures.

This plugin works only with the notation [release](https://github.com/notaryproject/notation/releases/tag/feat-kv-extensibility) built from the [feat-kv-extensibility](https://github.com/notaryproject/notation/tree/feat-kv-extensibility) feature branch.

## Getting Started

The following bash script block summaries the steps to configure the `cose` plugin, sign and verify a container image against COSE signatures.

```bash
# Configure notation with the COSE plugin
notation plugin add cose ~/.config/notation/plugins/cose/notation-cose

# Add signing and verification keys to the notation configuration policy
KEY_INFO="$KEY_PATH:${CERT_PATH}"
# Uncomment below for configuring timestamp server
# KEY_INFO="${KEY_INFO}:${TSA_URL}"
notation key add --name ${KEY_NAME} --plugin cose --id ${KEY_INFO} --kms
notation cert add --name ${KEY_NAME} --plugin cose --id ${CERT_PATH} --kms

# Sign image and generate COSE signature
notation sign --key ${KEY_NAME} ${IMAGE}

# Verify image against the COSE signature generated above
notation verify --cert ${KEY_NAME} ${IMAGE}
```

## Sample COSE Signature

A COSE signature generated by the `notation-cose` plugin printed using [cq](tools/cq) looks like

```
$ cat sample.sig | cq
d2 -- Tag 18: cose-sign1
84 -- COSE_Sign1 object: Array of length 4
   59 03 58 -- protected: Binary string: 856 bytes
      a5 -- Map of size 5
         01       -- Key:   Integer: 1
         38 24    -- Value: Integer: -37
         02       -- Key:   Integer: 2
         81       -- Value: Array of length 1
            03 -- Integer: 3
         03       -- Key:   Integer: 3
         78 35    -- Value: UTF-8 text: 53 bytes
            61 70 70 6c 69 63 61 74 69 6f 6e 2f 76 6e 64 2e -- application/vnd.
            63 6e 63 66 2e 6f 72 61 73 2e 61 72 74 69 66 61 -- cncf.oras.artifa
            63 74 2e 64 65 73 63 72 69 70 74 6f 72 2e 76 31 -- ct.descriptor.v1
            2b 6a 73 6f 6e                                  -- +json
         18 21    -- Key:   Integer: 33
         81       -- Value: Array of length 1
            59 03 02 -- Binary string: 770 bytes
               30 82 02 fe 30 82 01 e6 a0 03 02 01 02 02 11 00 -- 0...0...........
               af ba 5c 63 66 e1 c1 59 95 91 4a 95 cd 26 cf c6 -- ..\cf..Y..J..&..
               30 0d 06 09 2a 86 48 86 f7 0d 01 01 0b 05 00 30 -- 0...*.H........0
               14 31 12 30 10 06 03 55 04 03 0c 09 63 6f 73 65 -- .1.0...U....cose
               5f 74 65 73 74 30 1e 17 0d 32 32 30 32 31 35 30 -- _test0...2202150
               37 35 38 30 32 5a 17 0d 32 33 30 32 31 35 30 37 -- 75802Z..23021507
               35 38 30 32 5a 30 14 31 12 30 10 06 03 55 04 03 -- 5802Z0.1.0...U..
               0c 09 63 6f 73 65 5f 74 65 73 74 30 82 01 22 30 -- ..cose_test0.."0
               0d 06 09 2a 86 48 86 f7 0d 01 01 01 05 00 03 82 -- ...*.H..........
               01 0f 00 30 82 01 0a 02 82 01 01 00 bf 6f 1b 78 -- ...0.........o.x
               b9 52 98 7a 31 74 00 92 6b 3e 83 4a 6e ce cc 9b -- .R.z1t..k>.Jn...
               1d ac 3d fd 48 24 2a 0d f9 b6 d8 53 11 d2 67 af -- ..=.H$*....S..g.
               14 03 3e e6 d6 06 28 88 17 ed 68 31 48 e2 d8 ab -- ..>...(...h1H...
               dc 64 49 dd 00 a0 7d 68 e9 3f 7f 46 34 c5 81 ee -- .dI...}h.?.F4...
               79 fb e0 a9 4a 65 ec 49 f5 2b 7a c8 8b d4 6c 82 -- y...Je.I.+z...l.
               85 d8 18 ad ff c8 f7 d6 3c 2b 03 08 b8 da a7 f3 -- ........<+......
               c2 00 84 99 a8 0d cf ec e5 65 e1 a7 7c 5b 60 0d -- .........e..|[`.
               4c 97 52 f5 f8 89 5e 3d e1 8b 17 8e 6d 2b d1 cf -- L.R...^=....m+..
               be 7a 10 09 3c 7c 5f b6 2d e9 65 69 d1 61 19 65 -- .z..<|_.-.ei.a.e
               c2 23 73 43 d0 70 58 47 b9 25 88 ce cf ce 91 f8 -- .#sC.pXG.%......
               e4 fe fe d0 b3 e1 35 4a 89 09 6d d4 68 b1 74 c0 -- ......5J..m.h.t.
               86 34 03 70 7b 9a 94 15 e3 33 13 4a de fb f5 24 -- .4.p{....3.J...$
               7e de 07 70 05 4f 0d 50 f0 7f 78 22 b7 79 e9 be -- ~..p.O.P..x".y..
               e7 dc ae 7f be 0e 28 cc 1e 77 13 c2 9d 41 62 ad -- ......(..w...Ab.
               63 67 49 95 c1 0a 28 ed 2e 1b fd 04 22 c3 96 8f -- cgI...(....."...
               4c 36 88 2b 18 25 22 51 b2 19 d1 37 02 03 01 00 -- L6.+.%"Q...7....
               01 a3 4b 30 49 30 0e 06 03 55 1d 0f 01 01 ff 04 -- ..K0I0...U......
               04 03 02 07 80 30 13 06 03 55 1d 25 04 0c 30 0a -- .....0...U.%..0.
               06 08 2b 06 01 05 05 07 03 03 30 0c 06 03 55 1d -- ..+.......0...U.
               13 01 01 ff 04 02 30 00 30 14 06 03 55 1d 11 04 -- ......0.0...U...
               0d 30 0b 82 09 63 6f 73 65 5f 74 65 73 74 30 0d -- .0...cose_test0.
               06 09 2a 86 48 86 f7 0d 01 01 0b 05 00 03 82 01 -- ..*.H...........
               01 00 9e 35 c0 3b f8 f3 85 fc 56 56 73 68 e7 bd -- ...5.;....VVsh..
               2d 13 76 3c a8 35 12 1e 5e 22 a6 d0 f4 a8 1b 44 -- -.v<.5..^".....D
               a5 d9 eb c1 0c 88 0c cd bf f7 fe 70 4d 7c 6c 2e -- ...........pM|l.
               eb 78 c2 51 18 77 de 92 35 7c 45 09 53 92 c1 2d -- .x.Q.w..5|E.S..-
               00 6e b9 cb 36 d2 0f 9a 8e 10 fe ea 2d e3 9e b4 -- .n..6.......-...
               35 8b 0d 23 ab a0 31 a0 67 4c 35 7d e8 36 7f a2 -- 5..#..1.gL5}.6..
               4f d1 2b 14 c3 f3 90 17 42 f2 b0 a1 f7 51 87 01 -- O.+.....B....Q..
               2e a7 a4 4b 44 14 48 38 eb a2 78 5f bc 43 43 aa -- ...KD.H8..x_.CC.
               67 9f 3b bc 9a 3a 5d b3 04 26 78 6a 34 7c 22 be -- g.;..:]..&xj4|".
               a2 46 42 51 8a 3b fd b5 31 c1 2b ed 4a b7 8a a2 -- .FBQ.;..1.+.J...
               e4 5f 8d 55 2b 89 55 b7 de a2 20 09 93 da cf f8 -- ._.U+.U... .....
               6b b7 9d 85 ad c2 34 db ba fe fa 7f 55 4e 36 db -- k.....4.....UN6.
               3f 67 16 8d a4 c4 e8 80 6b 9e 27 42 98 ea 7f 46 -- ?g......k.'B...F
               39 76 71 89 ba 28 52 90 64 03 18 0a 1e 38 41 06 -- 9vq..(R.d....8A.
               b8 36 01 b6 55 f0 a4 e4 70 ba ee b6 0d 5a 09 4d -- .6..U...p....Z.M
               c8 52 47 78 0f c7 07 ed 42 0e c6 7f 84 1b 47 8a -- .RGx....B.....G.
               87 b1                                           -- ..
         6b       -- Key:   UTF-8 text: 11 bytes
            73 69 67 6e 69 6e 67 74 69 6d 65                -- signingtime
         1a 62 0f 92 a9 -- Value: Integer: 1645187753
   a0 -- unprotected: Map of size 0
   58 a2 -- payload: Binary string: 162 bytes
      7b 22 6d 65 64 69 61 54 79 70 65 22 3a 22 61 70 -- {"mediaType":"ap
      70 6c 69 63 61 74 69 6f 6e 2f 76 6e 64 2e 64 6f -- plication/vnd.do
      63 6b 65 72 2e 64 69 73 74 72 69 62 75 74 69 6f -- cker.distributio
      6e 2e 6d 61 6e 69 66 65 73 74 2e 76 32 2b 6a 73 -- n.manifest.v2+js
      6f 6e 22 2c 22 64 69 67 65 73 74 22 3a 22 73 68 -- on","digest":"sh
      61 32 35 36 3a 36 35 62 33 61 38 30 65 62 65 37 -- a256:65b3a80ebe7
      34 37 31 62 65 65 63 62 63 30 39 30 63 35 62 32 -- 471beecbc090c5b2
      63 64 64 30 61 61 66 65 61 65 66 61 30 37 31 35 -- cdd0aafeaefa0715
      66 38 66 31 32 65 34 30 64 63 39 31 38 61 33 61 -- f8f12e40dc918a3a
      37 30 65 33 32 22 2c 22 73 69 7a 65 22 3a 35 32 -- 70e32","size":52
      38 7d                                           -- 8}
   59 01 00 -- signature: Binary string: 256 bytes
      2f d3 60 41 8a 85 bc 3b 78 cd db db 5f 2f 18 41 -- /.`A...;x..._/.A
      f5 cb f2 72 3d c5 76 31 7a a2 a1 b5 6f 91 60 b4 -- ...r=.v1z...o.`.
      59 5d 47 ce 31 09 06 fe 41 49 31 5b 07 23 0a 6b -- Y]G.1...AI1[.#.k
      77 26 5d d0 a1 f7 4b 8b ed ab 13 fe 7e 5c 11 54 -- w&]...K.....~\.T
      54 c9 4b 40 10 ee 1e d4 ef fb cd 77 5b e7 01 fc -- T.K@.......w[...
      34 26 ac 14 51 6b fb cf d2 bd 15 59 d2 9b 5c 06 -- 4&..Qk.....Y..\.
      46 a2 23 f7 7d 18 ab c3 ef 25 a8 1a de fb ea 20 -- F.#.}....%..... 
      7d 9f 34 59 d2 87 26 8e 4b 0b c9 a5 77 3e a2 2e -- }.4Y..&.K...w>..
      d2 86 fd 7a 4c 47 1c c3 97 b1 96 3a f7 36 e4 8b -- ...zLG.....:.6..
      ed d1 1d 04 85 d1 c7 a2 70 27 5b 57 0a 37 8e 01 -- ........p'[W.7..
      df 89 7f 59 df f7 88 da c6 53 ca df 4a e2 ec 73 -- ...Y.....S..J..s
      b0 66 99 ab 47 f2 ba f3 be f6 46 19 82 bc d5 1e -- .f..G.....F.....
      ef a7 e7 ff ce 29 b4 49 86 a2 e8 01 ad 53 e4 b4 -- .....).I.....S..
      a0 3d 2c f6 bf ce cc 4d ad 30 48 f7 90 24 90 c4 -- .=,....M.0H..$..
      eb d6 5f 40 cd d0 91 10 f0 35 db 2e 9b d1 e5 10 -- .._@.....5......
      79 65 85 71 96 c5 3d 7e e8 43 ca 71 72 8f a6 a9 -- ye.q..=~.C.qr...
```
