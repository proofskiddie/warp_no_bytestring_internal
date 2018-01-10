# Warp less ByteString Internal

A reworking of yesodweb/warp with less dependency on Data.ByteString.Internal

Warp is a server library for HTTP/1.x and HTTP/2 based WAI(Web Application Interface in Haskell). For more information, see [Warp](http://www.aosabook.org/en/posa/warp.html).

Changes :
--------------

Network/Wai/Handler/Warp/Buffer.hs :
-------------------------------------

copy :: Buffer -> ByteString -> IO Buffer
mallocBS :: Int -> IO ByteString
bufferIO :: Buffer -> Int -> (ByteString -> IO ()) -> IO ()

*withForeignBuffer :: ByteString -> ((Buffer, BufSize) -> IO Int) -> IO Int
 left with dependence to Data.ByteString.Internal

Network/Wai/Handler/Warp/PackInt.hs :
-------------------------------------

packIntegral :: Integral a => a -> ByteString
packStatus :: H.Status -> ByteString

Network/Wai/Handler/Warp/ResponseHeader.hs :
--------------------------------------------

composeHeader :: H.HttpVersion -> H.Status -> H.ResponseHeaders -> IO ByteString
copyStatus :: H.HttpVersion -> H.Status -> ByteString
copyHeaders :: [H.Header] -> ByteString
copyHeader :: H.Header -> ByteString
copyCRLF :: ByteString -> ByteString

Network/Wai/Handler/Warp/RequestHeader.hs :
-------------------------------------------

parseHeaderLines :: [ByteString]
                 -> IO (H.Method
                       ,ByteString  --  Path
                       ,ByteString  --  Path, parsed
                       ,ByteString  --  Query
                       ,H.HttpVersion
                       ,H.RequestHeaders
                       )
parseRequestLine :: ByteString
                  -> IO (H.Method
                        ,ByteString -- Path
                        ,ByteString -- Query
                        ,H.HttpVersion)
