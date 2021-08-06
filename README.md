# Wordpress-codigos-snippets-uteis
Um conjunto de códigos snippets (scriptzinhos) úteis para wordpress
-------------------------------------------------------------------

# Woocommerce Cancelamento automático de pedidos com método de pagamento offline (transferência / boleto) em 5 dias após a data do pedido - Alteração de Aguardando (on-hold) para Cancelado (cancelled).

/**
 * AUTOCANCELAMENTO DE PEDIDOS COM O STATUS AGUARDANDO POR MAIS DE 5 DIAS
 * */
function autocancel_wc_orders(){
	$query = ( array(
		'limit'   => 200,
		'orderby' => 'date',
		'order'   => 'DESC',
		'status'  => array( 'on-hold' )
	) );
	$orders = wc_get_orders( $query );
	foreach( $orders as $order ){		

		$date     = new DateTime( $order->get_date_created() );
		$today    = new DateTime();
		$interval = $date->diff($today);
	
		$datediff = $interval->format('%a');

		if( $datediff >= 5 ){
			$order->update_status('cancelled', 'Cancelled for missing payment');
		}
		
	}

}
add_action( 'admin_init', 'autocancel_wc_orders' );

*IMPORTANTE: Limit (refere-se ao número de pedidos que cancelará por vez. utilizar um número pequeno para uma melhor performance e evitar erros.
................................................................................................................................................





