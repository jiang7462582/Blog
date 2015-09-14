# &lt;Github Page>Spray-client Rest
First:  sbt import
<pre>"io.spray" %% "spray-client" % "1.3.1",
"io.spray" %% "spray-can" % "1.3.2",
"io.spray" %% "spray-routing" % "1.3.2"</pre>
***

Second : code Example
*Get request*

<pre>import spray.client.pipelining._
import spray.http._
import scala.concurrent._
import akka.actor.{ActorSystem, Props}
import scala.concurrent.ExecutionContext.Implicits.global
import scala.util._
implicit val system = ActorSystem()
val pipeline: HttpRequest => Future[HttpResponse] = sendReceive
val response: Future[HttpResponse] = pipeline(Get("http://tiku.huatu.com/apis/cascalog/logs/mod_user_log"))
response.onComplete {
  case Failure(t) =>
    println(s"error ")
  case Success(r) =>
    	pintln(s"success ${r.status}")
    println(r.entity.asString)
}</pre>


 *Post request*

<pre>case class User(id: Option[Int], username: String)

implicit val orderConfirmationFormat = jsonFormat2(User)   // extends DefaultJsonProtocol
mport spray.client.pipelining._
import spray.http._
import scala.concurrent._
import akka.actor.{ActorSystem}
import scala.concurrent.ExecutionContext.Implicits.global
import scala.util._
implicit val system = ActorSystem()
import spray.httpx.SprayJsonSupport._  //重要
val pipeline: HttpRequest => Future[User] =   sendReceive ~> unmarshal[User]
val response: Future[User] = pipeline { Post("http://localhost:8088/users", User(Option(100),"jiang"))}
  response.onComplete{
    case Failure(t) =>
      println(s"error $t")
    case Success(r) =>
      println(s"success ${r}")
      println(r)
  }</pre>

**完整代码请参考**[Gist Code](https://gist.github.com/jiang7462582/6d42f8393e6c9676aee7) 

