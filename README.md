# fs2-cron
[![Build Status](https://travis-ci.org/fthomas/fs2-cron.svg?branch=master)](https://travis-ci.org/fthomas/fs2-cron)
[![codecov](https://codecov.io/gh/fthomas/fs2-cron/branch/master/graph/badge.svg)](https://codecov.io/gh/fthomas/fs2-cron)

## Quick example

```scala
import cats.effect.IO
import cron4s.Cron
import eu.timepit.fs2cron._
import fs2.{Scheduler, Stream}
import java.time.LocalDateTime
import scala.concurrent.ExecutionContext.Implicits._
```
```scala
val evenSeconds = Cron.unsafeParse("*/2 * * ? * *")
// evenSeconds: cron4s.expr.CronExpr = */2 * * ? * *

val stream = Scheduler[IO](1).flatMap {
  _.awakeEveryCron[IO](evenSeconds) >> Stream.eval(IO(println(LocalDateTime.now)))
}
// stream: fs2.Stream[cats.effect.IO,Unit] = Stream(..)

stream.take(3).compile.drain.unsafeRunSync
// 2018-08-26T07:40:38.094
// 2018-08-26T07:40:40.003
// 2018-08-26T07:40:42.005
```

## License

**fs2-cron** is licensed under the Apache License, Version 2.0, available at
http://www.apache.org/licenses/LICENSE-2.0 and also in the
[LICENSE](https://github.com/fthomas/status-page/blob/master/LICENSE) file.
