# autoeur-usd
forex
//@version=4
strategy("Mi Semaforin", overlay = true)

loingitud_mm_v = input(x, title= "longitud media movil verde")
loingitud_mm_a = input(x, title= "longitud media movil amarillo")
loingitud_mm_r = input(x, title= "longitud media movil rojo")

pintar_mm = input(false,title= "pintar medias moviles" )

//desde
desde_a = input( 2000,title= "desde año")
desde_m = input( 1,title= "desde mes", minval =1, maxval=12)
desde_d = input( 1,title= "desde dia") minval =1, maxval=31)

hasta_a = input( 2030,title= "hasta año")
hasta_m = input( 1,title= "hasta", minval=1, maxval=12)
hasta_d = input( 1,title= "hasta", minval=1, maxval=31)

FechaValida()=>
 desde = time >= timestamp(syminfo.timezone, desde_a, desde_m, desde_d,0,0 )
hasta = time >= timestamp(syminfo.timezone, hasta, hasta_m, hasta_d,0,0 )
desde and hasta


mm_v = sma(close,5)
mm_a = sma(close, 10)
mm_r = sma(close,20)

alcista = (mm_v > mm_a) and (mm_a > mm_r)

comprado = strategy.position_size > 0

if (not comprado and alcista and FechaValida() )
// realizar compra 
cantidad = round strategy.equity / close)
strategy.entry("compra", strategy.long,cantidad )
if (comprado and not alcista)
//realizar venta
strategy.close ("compra", comment= "venta")

if (comprado and not FechaValida())
//relizae venta por dinalizacion de periodo
strategy.close("compra" ,comment = "Venta x fin")

plot( pintar_mm   ? mmv : na,color=color.green)
plot( pintar_mm   ? mma : na,color=color.yellow)
plot( pintar_mm   ? mmr : na,color=color.red)
