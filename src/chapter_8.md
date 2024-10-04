# Chapter 8 - プロトコル

本来はもっと細かくステップを踏む予定でしたが、効率が悪いので私が過去に書いたコードの解説そしていこうと思います。

- [GitHub](https://github.com/TUAT-RUR/RoboCon2024)
- [GitLab](https://rur.mech.tuat.ac.jp/internal/gitlab/nhk2024/RoboCon2024)

どちらも権限が必要ですが、同じコードですので見られる方を参照してください。
また、コードや設計は汚い部分がありますが、ご容赦ください。

## プロトコルの定義(適当)

実際に作成した際は、ROS 2 の方で先に C++ でプロトコルを定義して、ChatGPT と一緒に C# で再実装しました。

`Assets/RoboCon2024/Scripts/Network/Packet.cs` にプロトコルを定義しています。

先頭から、8bitのバージョン、8bitのドメインID、8bitのタイプ、16bitのデータ長、データ本体の構造体を持っています。以下は、このプロトコルを作成する際に考えていたことです。

- **バージョン**: 開発の進捗によっては新旧のプロトコルが混在する可能性があるので、バージョンを持たせることで互換性を保つことができます。(そもそも実用化が遅すぎて出番はなかったけれども)
- **NHK2024**: では手動機が1台であったため、ドメインIDはほとんど意味をなしていませんが、複数台のロボットを用いる際には必要になると思われます。
- **タイプ**: パケットの種類を表します。受信者はこのタイプを見て、どのようにデータを処理するかを決定します。
- データ長: 

当初はブロードキャストを用いてスリーウェイハンドシェイクの様なことをし、通信の確立以降はユニキャストしようと考えていましたが、切断時の処理が困難だったため、一旦

```csharp
sing System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

namespace RobotController.Network
{
    public class Packet
    {
        // Constants
        public const byte Version = 0x01;

        public enum Type : byte
        {
            JoyconData = 0x01,
            JoyconConnectionCheck = 0x02,
            OdometryData = 0x04,
            SequenceData = 0x06,
        }

        // Main structure
        public struct Header
        {
            public byte version;
            public byte domainId;
            public Type type;
            public ushort dataLength;
        }

        public struct Body
        {
            public List<byte> data;
        }

        // Constructors
        public Packet()
        {
            _header = new Header();
            _body = new Body();
        }

        public Packet(Header header, Body body)
        {
            _header = header;
            _body = body;
        }

        // Header and Data accessors
        public void SetHeader(Header header)
        {
            _header = header;
        }

        public void SetBody(Body body)
        {
            _body = body;
        }

        public Header GetHeader()
        {
            return _header;
        }

        public Body GetBody()
        {
            return _body;
        }

        // Serialization and Deserialization
        public List<byte> Serialize()
        {
            List<byte> serialized = new()
            {
                _header.version,
                _header.domainId,
                (byte)_header.type,
                (byte)(_header.dataLength & 0xFF),
                (byte)((_header.dataLength >> 8) & 0xFF)
            };
            serialized.AddRange(_body.data);
            return serialized;
        }

        public void Deserialize(IReadOnlyList<byte> serialized)
        {
            if (serialized.Count < HeaderSize)
            {
                throw new Exception($"Invalid packet size: {serialized.Count}");
            }

            // Debug.Log($"serialized.Count: {serialized.Count}");
            // Debug.Log($"Data: {BitConverter.ToString(serialized.ToArray())}");
            _header.version = serialized.ElementAt(0);
            _header.domainId = serialized.ElementAt(1);
            _header.type = (Type)serialized.ElementAt(2);
            _header.dataLength = BitConverter.ToUInt16(serialized.Skip(3).Take(2).ToArray());
            // Debug.Log(
            //     $"version: {(int)_header.version}, domainId: {(int)_header.domainId}, type: {_header.type}, dataLength: {_header.dataLength}");
            _body.data = serialized.Skip(HeaderSize).Take(_header.dataLength).ToList();
        }

        // Private members
        private Header _header;
        private Body _body;
        private const int HeaderSize = 5;
    }
}
```
