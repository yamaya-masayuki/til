# Perfect Forward Secrecy - 完全前方秘匿性

PFS (Perfect Forward Secrecy)とは暗号化された通信と暗号化するための秘密鍵が両方漏洩しても復号化できない、という鍵交換プロトコル上の性質である。

## SSL/TLSにおけるforward secrecy

SSLで前方秘匿性を実現可能なアルゴリズムとして、ディフィー・ヘルマン鍵共有(DHE)と、それを楕円曲線暗号で実現した、楕円曲線ディフィー・ヘルマン鍵共有(ECDHE)が存在する。
