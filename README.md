# Warp less ByteString Internal

A reworking of yesodweb/warp with less dependency on Data.ByteString.Internal

Warp is a server library for HTTP/1.x and HTTP/2 based WAI(Web Application Interface in Haskell). For more information, see [Warp](http://www.aosabook.org/en/posa/warp.html).

Changes :
--------------

Network/Wai/Handler/Warp/Buffer.hs :
-------------------------------------

1. copy :: Buffer -> ByteString -> IO Buffer
2. mallocBS :: Int -> IO ByteString
3. bufferIO :: Buffer -> Int -> (ByteString -> IO ()) -> IO ()

*4. withForeignBuffer :: ByteString -> ((Buffer, BufSize) -> IO Int) -> IO Int
 left with dependence to Data.ByteString.Internal

Network/Wai/Handler/Warp/PackInt.hs :
-------------------------------------

1. packIntegral :: Integral a => a -> ByteString
2. packStatus :: H.Status -> ByteString

Network/Wai/Handler/Warp/ResponseHeader.hs :
--------------------------------------------

1. composeHeader :: H.HttpVersion -> H.Status -> H.ResponseHeaders -> IO ByteString
2. copyStatus :: H.HttpVersion -> H.Status -> ByteString
3. copyHeaders :: [H.Header] -> ByteString
4. copyHeader :: H.Header -> ByteString
5. copyCRLF :: ByteString -> ByteString

Network/Wai/Handler/Warp/RequestHeader.hs :
-------------------------------------------

1. parseHeaderLines :: [ByteString]
                 -> IO (H.Method
                       ,ByteString  --  Path
                       ,ByteString  --  Path, parsed
                       ,ByteString  --  Query
                       ,H.HttpVersion
                       ,H.RequestHeaders
                       )
2. parseRequestLine :: ByteString
                  -> IO (H.Method
                        ,ByteString -- Path
                        ,ByteString -- Query
                        ,H.HttpVersion)
