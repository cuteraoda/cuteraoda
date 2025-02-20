<!--{if $raku2param.favorite_use_storage == 1}-->
	<!--{if $favorite_flg || $tpl_dairi_login}-->
		<script>
		$(function(){
			let arrFav = new Array();
			<!--{foreach from=$arrFavoriteLs item=item key=key}-->
				arrFav.push({
					id : <!--{$item.product_id}-->
				});
			<!--{/foreach}-->

		<!--{if $tpl_dairi_login}-->
			localStorage.removeItem("raku2_favorites");
			localStorage.removeItem("raku2_favorites_del");
		<!--{/if}-->
			localStorage.setItem("raku2_favorites", JSON.stringify(arrFav));
			localStorage.setItem("raku2_favorites_del", '');

			// 必要formのhidden項目にvalue再設定
			$('#favorites_ls').val(localStorage.getItem('raku2_favorites'));
			$('#favorites_del_ls').val(localStorage.getItem('raku2_favorites_del'));
		});
		</script>
	<!--{/if}-->
<!--{/if}-->

<!--{if $raku2param.raku2_other_order_view==1}-->
<script>
	window.onpageshow = function(){
		$(function(){
			if(document.getElementById("view_switch").checked) {
				$('.my_order').hide();
			} else {
				$('.other_order').hide();
			}
		});
	};
	$(function(){
		$('.switch__input').change(function(){
			$('.my_order').toggle();
			$('.other_order').toggle();
		})
	});
</script>

<style type="text/css">
	/*管理画面履歴用スイッチと同じ*/
	.switch {
		margin-bottom: 10px;
		float: right;
	}
	.switch__label {
		width: 40px;
		position: relative;
		display: inline-block;
	}
	.switch__content {
		display: block;
		cursor: pointer;
		position: relative;
		border-radius: 30px;
		height: 21px;
		overflow: hidden;
	}
	.switch__content:before {
		content: "";
		display: block;
		position: absolute;
		width: calc(100% - 3px);
		height: calc(100% - 3px);
		top: 0;
		left: 0;
		border: 1.5px solid #E5E5EA;
		border-radius: 30px;
		background-color: #fff;
	}
	.switch__content:after {
		content: "";
		display: block;
		position: absolute;
		background-color: transparent;
		width: 0;
		height: 0;
		top: 50%;
		left: 50%;
		border-radius: 30px;
		-webkit-transition: all .5s;
		-moz-transition: all .5s;
		-ms-transition: all .5s;
		-o-transition: all .5s;
		transition: all .5s;
	}
	.switch__input {
		display: none;
	}

	.switch__circle {
		display: block;
		top: 2px;
		left: 2px;
		position: absolute;
		-webkit-box-shadow: 0 2px 6px #999;
		box-shadow: 0 2px 6px #999;
		width: 17px;
		height: 17px;
		-webkit-border-radius: 20px;
		border-radius: 20px;
		background-color: #fff;
		-webkit-transition: all .5s;
		-moz-transition: all .5s;
		-ms-transition: all .5s;
		-o-transition: all .5s;
		transition: all .5s;
	}
	.switch__input:checked ~ .switch__circle {
		left: 21px;
	}

	.switch__input:checked ~ .switch__content:after {
		background-color: #87cefa;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
	}
</style>
<!--{/if}-->

<div id="mypagecolumn">
	<!--{if $raku2param.cart_b2b_simple_form == 0}-->
		<h2 class="title">
			<div><i class="fa fa-user"></i></div>
			<!--{$tpl_title|h}-->
			<div class="menber_name"><!--{$tpl_company_name|h}-->　様</div>
		</h2>
	<!--{else}-->
		<h2 class="title"><!--{$tpl_title|h}--></h2>
	<!--{/if}-->

	<div id="mycontents_area" class="mypage-top">
		<!--{include file=$tpl_navi}-->
		<!--{if $tpl_customer_type==constant("Raku2::CUSTOMER_TYPE_MEMBER")}-->
			<!--{$raku2param.message_pc_mypage_member_top_upper}-->
		<!--{elseif $tpl_customer_type==constant("Raku2::CUSTOMER_TYPE_MEMBER_GROUP")}-->
			<!--{$raku2param.message_pc_mypage_group_top_upper}-->
		<!--{else}-->
			<!--{if $raku2param.message_pc_mypage_top_upper!=""}-->
				<!--{$raku2param.message_pc_mypage_top_upper}-->
			<!--{else}-->
				<!--{include file="`$smarty.const.PLUGIN_UPLOAD_REALDIR`Raku2/view_template/message/raku2pc/placeholder/def_message_pc_mypage_top_upper.tpl"}-->
			<!--{/if}-->
		<!--{/if}-->
		
		<!--{if $raku2param.b2b2c_only=="0" && $tpl_customer_type==constant("Raku2::CUSTOMER_TYPE_KAIIN")  ||
				$raku2param.b2b2c_only=="1" && $tpl_customer_type==constant("Raku2::CUSTOMER_TYPE_MEMBER") ||
				$raku2param.b2b2c_only=="2" && $tpl_customer_type!=constant("Raku2::CUSTOMER_TYPE_MEMBER_GROUP")}-->
			<form name="form1" id="form1" method="post" action="?">
				<input type="hidden" name="<!--{$smarty.const.TRANSACTION_ID_NAME}-->" value="<!--{$transactionid}-->" />
				<input type="hidden" name="order_id" value="" />
				<input type="hidden" name="pageno" value="<!--{$objNavi->nowpage}-->" />
				<input type="hidden" name="previous_page" value="mypage_top">

				<!--{if $raku2param.raku2_other_order_view==1}-->
					<!--▼他メンバー表示用スイッチ-->
					<div class="switch">
						<label class="switch__label">
							<input type="checkbox" class="switch__input" id="view_switch"/>
							<span class="switch__content"></span>
							<span class="switch__circle"></span>
						</label>
					</div>
				<!--{/if}-->

				<div class="raku2_mypage_order_products_history">
					<h3>ご注文中の商品</h3>
					<div id="order_history">
						<!--{if $arrOrder|@count_php8 > 0}-->
							
							<!--{foreach from=$arrOrder item=item}-->
								
							<!--▼注文内容表示-->
							<!--{if $raku2param.raku2_other_order_view==1}-->
								<!--{if 0 == $member_staff_id}-->
									<!--{if $item.plg_raku2_b2b_member_staff_id == NULL}-->
										<div class="order_history_list my_order">
									<!--{else}-->
										<div class="order_history_list other_order">
									<!--{/if}-->
								<!--{else}-->
									<!--{if $item.plg_raku2_b2b_member_staff_id == $member_staff_id}-->
										<div class="order_history_list my_order">
									<!--{else}-->
										<div class="order_history_list other_order">
									<!--{/if}-->
								<!--{/if}-->
							<!--{else}-->
								<div class="order_history_list">
							<!--{/if}-->

								<!--{assign var=order_status_id value="`$item.plg_raku2_order_status`"}-->
								<!--▼注文内容ヘッダー部-->
								<div class="order_info">
									<div class="raku2_mypage_top_order_date">
										<p><span class="mini"><span class="raku2_mypage_top_order_date_label">注文日</span></span><br>
										<!--{$item.plg_raku2_order_date|plg_raku2_sfDispDBDate:"false":"true"}--></p>
									</div>
									<div class="raku2_mypage_top_order_hasso_date">
										<p><span class="mini"><span class="raku2_mypage_top_order_hasso_date_label">出荷予定日</span></span><br>
										<!--{$item.plg_raku2_hasso_date|plg_raku2_sfDispDBDate:"false":"true"}--></p>
									</div>

									<div class="raku2_mypage_top_order_subtotal">
										<p><span class="mini"><span class="raku2_mypage_top_order_subtotal_label"><!--{if $raku2param.mypage_subtotal_kbn=="1"}-->ご請求額<!--{else}-->商品代金合計<!--{/if}--></span></span><br>
										<!--{if $raku2param.mypage_subtotal_kbn=="1"}-->
											<!--{if !$item.calcMaskFlg}-->
												<!--{$item.payment_total|number_format_ex|h}-->円
											<!--{else}-->
												<!--{$raku2param.price_show_zero_msg|default:"***円"}-->
											<!--{/if}-->
										<!--{else}-->
											<!--{if $raku2param.mypage_subtotal_kbn=="2"}-->
												<!--{if !$item.calcMaskFlg}-->
													<!--{$item.subtotal-$item.tax|number_format_ex|h}-->円
												<!--{else}-->
													<!--{$raku2param.price_show_zero_msg|default:"***円"}-->
												<!--{/if}-->
											<!--{else}-->
												<!--{if !$item.calcMaskFlg}-->
													<!--{$item.subtotal|number_format_ex|h}-->円
												<!--{else}-->
													<!--{$raku2param.price_show_zero_msg|default:"***円"}-->
												<!--{/if}-->
											<!--{/if}-->
										<!--{/if}-->
										</p>
									</div>

									<div class="raku2_mypage_top_order_id">
										<p><span class="mini"><span class="raku2_mypage_top_order_id_label">注文番号</span></span><br>
										<!--{$item.order_ids}--></p>
									</div>
									
									<!--{if $item.plg_raku2_reserve_type == "3" && $raku2param.mypage_order_detail_reorder=="1" || $raku2param.mypage_order_detail_reorder=="2"}-->
										<a href="javascript:void(0);" onclick="eccube.changeAction('<!--{$smarty.const.ROOT_URLPATH}-->mypage/history.php?order_id=<!--{$item.order_id}-->', 'form1'); eccube.setValueAndSubmit('form1', 'order_id', '<!--{$item.order_id}-->'); return false;" class="nav_btn"><i class="fa fa-caret-right" aria-hidden="true"></i><span class="raku2_mypage_top_order_detail_btn"> 注文内容の詳細</span></a>
									<!--{else}-->
										<a href="javascript:void(0);" onclick="eccube.changeAction('<!--{$smarty.const.ROOT_URLPATH}-->mypage/history.php?order_id=<!--{$item.order_id}-->', 'form1'); eccube.setValueAndSubmit('form1', 'order_id', '<!--{$item.order_id}-->'); return false;" class="nav_btn"><i class="fa fa-caret-right" aria-hidden="true"></i><span class="raku2_mypage_top_order_detail_btn"> 注文内容の詳細、再注文</span></a>
									<!--{/if}-->
									
									<!--{if $raku2_owner_code=="kobayashimetals"}-->
									<div class="order_sender_wrap" style="font-size: 14px;">
										<div class="order_shipping">
											<p><span class="mini">注文者</span><br>
											<!--{$item.order_name01}--> <!--{$item.order_name02}--></p>
										</div>
										<div class="order_shipping">
											<!--{assign var=order_status_id value="`$item.plg_raku2_order_status`"}-->
											<p><span class="mini">ご注文状況</span><br>
											<span style="background-color:<!--{$arrCustomerOrderStatusColor[$order_status_id]|h}-->"><b><!--{$arrCustomerOrderStatus[$order_status_id]|h}--></b></span></p>
										</div>
									</div>
									<!--{/if}-->
									
									<!--{* 加盟店ユーザーの場合のみ、お届け先を表示する *}-->
									<!--{if $tpl_customer_type==constant("Raku2::CUSTOMER_TYPE_MEMBER")}-->
										<!--{if $item.plg_raku2_sender_customer_flg=="1"}-->
											<div class="order_sender_wrap">
												<div class="order_sender">
													<p>
														<span class="icon_ship">送り主</span>
														<!--{if strlen($item.order_trade_code) > 0}-->[<!--{$item.order_trade_code}-->]<!--{/if}-->
														<!--{if strlen($item.order_company_name) > 0}-->【<!--{$item.order_company_name}-->】<!--{/if}--><!--{$item.order_name01}--> <!--{$item.order_name02}--> 様（tel：<!--{$item.order_tel01}-->-<!--{$item.order_tel02}-->-<!--{$item.order_tel03}-->）</p>
												</div>
											</div>
										<!--{/if}-->

										<div class="order_shipping_wrap" <!--{if $item.pickupDisplay}-->style="display:none"<!--{/if}-->>
											<div class="order_shipping">
												<p><span class="icon_ship raku2-mr5">お届け先</span>
												<!--{if strlen($item.shipping_trade_code) > 0}-->[<!--{$item.shipping_trade_code}-->]<!--{/if}-->
												<!--{if strlen($item.shipping_company_name) > 0}-->【<!--{$item.shipping_company_name}-->】<!--{/if}--><!--{$item.shipping_name01}--> <!--{$item.shipping_name02}--> 様（tel：<!--{$item.shipping_tel01}-->-<!--{$item.shipping_tel02}-->-<!--{$item.shipping_tel03}-->）</p>
											</div>
										</div>

										<!--{if $raku2param.raku2_other_order_view==1}-->
										<!--{if $raku2_owner_code!="kobayashimetals"}-->
										<div class="estimate_shipping_wrap">
											<div class="estimate_shipping">
												<p><span class="icon_ship raku2-mr5">ご担当者</span>&nbsp;<!--{$arrMemberStaff[$item.plg_raku2_b2b_member_staff_id]}--></p>
											</div>
										</div>
										<!--{/if}-->
										<!--{/if}-->

									<!--{/if}-->
								</div>
								
								<!--▼注文内容詳細-->
								<div class="order_detail_info">
									<!--▼各種ステータス表示-->
									<!--{if $smarty.const.MYPAGE_ORDER_STATUS_DISP_FLAG}-->
										<!--{if $item.status == $smarty.const.ORDER_PENDING}-->
										<div class="order_status">
											<!-- 決済処理中(受注未確定)の場合のみ、赤文字表示 -->
											<span class="attention"><!--{$arrCustomerOrderStatus[$order_status_id]|h}--></span>
										</div>
										<!--{/if}-->
									<!--{/if}-->
									
									<!--{if $item.status == 3}-->
										<div class="order_status">
											<p class="order_status_cancel">キャンセル済み</p>
										</div>
									<!--{/if}-->
									
									<!--{* 受注ステータスが[発送ﾒｰﾙ待ち]～[処理済み] or 伝票番号が入力されていたら【出荷済み】文言表示 *}-->
									<!--{if $item.status == 5 || $item.status == 6 || $item.plg_raku2_hasso_denpyo_code != ""}-->
										<div class="order_status">
											<p class="deliv_status_close">出荷済み</p>
										</div>
									<!--{/if}-->
									
									<!--▼購入商品表示-->
									<div class="order_item">
									<!--{foreach from=$item.detail item=item_detail name="item_detail"}-->
									
										<div class="item_bloc">
											<!--{if $raku2param.cart_b2b_simple_form != 0}-->
											<!--▼商品画像-->
											<!--{if $raku2param.no_image_opt=="1" && $item_detail.main_large_image==""}-->
											<!--{else}-->
												<!--{assign var=products_path value=$item_detail.main_large_image|sfNoImageMainList}-->
												<!--{if strpos($item_detail.main_large_image, $smarty.const.EXTERNAL_URL) === false}-->
													<!--{assign var=products_path value=$smarty.const.IMAGE_SAVE_URLPATH|cat:$products_path}-->
												<!--{/if}-->
											
												<!--{if $raku2param.product_name_link2==0 || $item_detail.plg_raku2_limited_passwd!=''}-->
													<div class="raku2_item_image">
														<img src="<!--{$products_path|h}-->" alt="<!--{if strlen($item_detail.plg_raku2_main_large_image_alt) > 0}--><!--{$item_detail.plg_raku2_main_large_image_alt|h}--><!--{else}--><!--{$item_detail.product_name|h}--><!--{/if}-->" class="js-replace-no-image-mini">
													</div>
												<!--{else}-->
													<!--{if $raku2param.products_page_url_type==1}-->
														<a href="/products/detail/<!--{$item_detail.product_id}-->.html" target="_blank" class="item_image">
															<img src="<!--{$products_path|h}-->" alt="<!--{if strlen($item_detail.plg_raku2_main_large_image_alt) > 0}--><!--{$item_detail.plg_raku2_main_large_image_alt|h}--><!--{else}--><!--{$item_detail.product_name|h}--><!--{/if}-->" class="js-replace-no-image-mini" />
														</a>
													<!--{elseif $raku2param.products_page_url_type==2}-->
														<a href="/products/<!--{$item_detail.plg_raku2_common_code|escape|urlencode}-->/" target="_blank" class="item_image">
															<img src="<!--{$products_path|h}-->" alt="<!--{if strlen($item_detail.plg_raku2_main_large_image_alt) > 0}--><!--{$item_detail.plg_raku2_main_large_image_alt|h}--><!--{else}--><!--{$item_detail.product_name|h}--><!--{/if}-->" class="js-replace-no-image-mini" />
														</a>
													<!--{else}-->
														<a href="/products/detail.php?product_id=<!--{$item_detail.product_id}-->" target="_blank" class="item_image">
															<img src="<!--{$products_path|h}-->" alt="<!--{if strlen($item_detail.plg_raku2_main_large_image_alt) > 0}--><!--{$item_detail.plg_raku2_main_large_image_alt|h}--><!--{else}--><!--{$item_detail.product_name|h}--><!--{/if}-->" class="js-replace-no-image-mini" />
														</a>
													<!--{/if}-->
												<!--{/if}-->
											<!--{/if}-->
											<!--{/if}-->

											<!--▼商品名、価格、レビュー記入状況-->
											<div class="item_detail">
												<!--{* 定期アイコン *}-->
												<!--{if $item_detail.plg_raku2_product_reg_flag=='1'}-->
													<p><img src="<!--{$TPL_URLPATH}-->img/icon/ico_08.gif" alt="定期購入" width="60" /></p>
												<!--{/if}-->

												<p class="raku2_break_word">
													<!--{* 商品名 *}-->
													<!--{if $raku2param.product_name_link2==0 || $item_detail.plg_raku2_limited_passwd!=''}-->
														<!--{$item_detail.product_name}-->
													<!--{else}-->
														<!--{if $raku2param.products_page_url_type==1}-->
															<a href="/products/detail/<!--{$item_detail.product_id}-->.html" target="_blank"><!--{$item_detail.product_name}--></a><br>
														<!--{elseif $raku2param.products_page_url_type==2}-->
															<a href="/products/<!--{$item_detail.plg_raku2_common_code|escape|urlencode}-->/" target="_blank"><!--{$item_detail.product_name}--></a><br>
														<!--{elseif $raku2param.products_page_url_type==3}-->
															<a href="/products/products_list.php?product_id=<!--{$item_detail.product_id}-->&sitem=<!--{$item_detail.plg_raku2_common_code|escape|urlencode}-->"><!--{$item_detail.product_name}--></a><br>
														<!--{else}-->
															<a href="/products/detail.php?product_id=<!--{$item_detail.product_id}-->" target="_blank"><!--{$item_detail.product_name}--></a><br>
														<!--{/if}-->
													<!--{/if}-->

													<!--{* 規格名 *}-->
													<!--{if $item_detail.plg_raku2_product_option_name|h}--><p class="mini"><!--{$item_detail.plg_raku2_product_option_name|h}--></p><!--{/if}-->

													<!--{* のし名 *}-->
													<!--{if $raku2param.noshi_order_type=="2" || $raku2param.noshi_order_type=="3"}-->
														<!--{if $raku2param.noshi_select_regist==1}-->
															<!--{if $item_detail.plg_raku2_product_noshi_select|h}-->
																<p class="mini"><!--{$raku2param.noshi_select_label|default:"のし区分（お歳暮、粗品、内祝い 等）"}-->:
																	<!--{if $raku2param.noshi_select_type=="1"}-->
																		<!--{if $item_detail.plg_raku2_product_noshi_select!=""}-->
																			<!--{$arrNoshiSelect[$item_detail.plg_raku2_product_noshi_select]}-->
																		<!--{else}-->
																			希望しない
																		<!--{/if}-->
																	<!--{else}-->
																		<!--{$item_detail.plg_raku2_product_noshi_select|h}-->
																	<!--{/if}-->
																</p>
															<!--{/if}-->
														<!--{/if}-->
														<!--{if $raku2param.noshi_name_regist==1}-->
															<!--{if $item_detail.plg_raku2_product_noshi_name|h}-->
																<p class="mini"><!--{$raku2param.noshi_name_label|default:"名入れ"}-->:<!--{$item_detail.plg_raku2_product_noshi_name|h}--></p>
															<!--{/if}-->
														<!--{/if}-->
													<!--{/if}-->
<!--{if $license.raku_raku2cube_opt == 2}-->
													<p class="base_price">
														<!--{if !$item_detail.calcMaskFlg}-->
															<!--{if $item_detail.plg_raku2_tax_flag=="1" && $raku2param.mypage_subtotal_kbn!="2"}-->
																<!--{* 税込価格 *}-->
																<!--{$item_detail.product_price_inctax|number_format_ex:$raku2param.product_less_than_decimal}-->円
															<!--{else}-->
																<!--{* 税別価格 *}-->
																<!--{$item_detail.product_price_notax|number_format_ex:$raku2param.product_less_than_decimal}-->円
															<!--{/if}-->

															<!--{if $item_detail.plg_raku2_price_flg=="1"}-->
																<small>(<!--{$raku2param.price02_label_1|default:"会員価格"}-->)</small>
															<!--{/if}-->
															<!--{if $item_detail.plg_raku2_price_flg=="2"}-->
																<small>(初回価格)</small>
															<!--{/if}-->
															<!--{if $item_detail.plg_raku2_price_flg=="3"}-->
																<small>(<!--{$raku2param.price02_label_3|default:"加盟店価格"}-->)</small>
															<!--{/if}-->

															<!--{if $raku2param.mypage_order_b2b_trade_rate_flg=="1"}-->
																<!--{* 掛け率 *}-->
																<!--{if ($item_detail.plg_raku2_price_flg=="3" && $item_detail.plg_raku2_b2b_trade_rate>0 && $item_detail.plg_raku2_b2b_trade_rate!=100 && $item_detail.plg_raku2_b2b_trade_rate>0)}-->
																	<p class="trade_rate"><small>(<!--{$item_detail.plg_raku2_base_price02|number_format_ex:$raku2param.product_less_than_decimal}-->円×<!--{$item_detail.plg_raku2_b2b_trade_rate|number_format_ex}-->%)</small></p>
																<!--{/if}-->
															<!--{/if}-->

														<!--{else}-->
															<!--{$raku2param.price_show_zero_msg|default:"***円"}-->
														<!--{/if}-->

														<!--{* 数量 *}-->
														<span class="history_quantity">(数量：<!--{$item_detail.quantity}-->)</span>
													</p>
<!--{else}-->
													<!--{if $license.raku_teiki_course!=1}-->
														<!--{* 税込価格 *}-->
														<!--{$item_detail.product_price_inctax|number_format_ex:$raku2param.product_less_than_decimal}-->円
													<!--{/if}-->

													<!--{* 数量 *}-->
													<span class="history_quantity">(数量：<!--{$item_detail.quantity}-->)</span>
<!--{/if}-->
												</p>
												<!--{* レビュー記入状況 *}-->
												<!--{if $raku2param.review_write_flg != 4}-->
													<!--{if $item_detail.review_ok_flg}-->
														<!--{if $item_detail.review.comment==""}-->
															<p class="review_btn"><span class="mini"><a href="../products/review.php?order_id=<!--{$item.order_id}-->&product_id=<!--{$item_detail.product_id}-->&product_class_id=<!--{$item_detail.product_class_id}-->"><i class="fa fa-comment-o" aria-hidden="true"></i> 商品レビューを書く</a></span></p>
														<!--{else}-->
															<p class="review_add_mark"><span class="mini"><i class="fa fa-comment" aria-hidden="true"></i> 商品レビュー記入済み</span></p>
														<!--{/if}-->
													<!--{/if}-->
												<!--{/if}-->
											</div>
										</div>
									<!--{/foreach}-->
									</div>
									
									<!--▼各種ナビゲーションエリア-->
									<div class="order_nav_area">
										<!--{* 伝票番号の登録あればリンク表示 *}-->
										<!--{if $item.plg_raku2_hasso_denpyo_code != ""}-->
											<!--{assign var='arr_hasso_denpyo_code' value=","|explode:$item.plg_raku2_hasso_denpyo_code}-->
											<!--{foreach name=arr_hasso_denpyo_code from=$arr_hasso_denpyo_code item=hasso_denpyo_code}-->
												<!--{if $smarty.foreach.arr_hasso_denpyo_code.index>0}-->
													<!--{assign var='hasso_denpyo_code_no' value=$smarty.foreach.arr_hasso_denpyo_code.index+1}-->
												<!--{else}-->
													<!--{assign var='hasso_denpyo_code_no' value=''}-->
												<!--{/if}-->
												
												<!--{if $item.plg_raku2_hasso_deliv_id == "1001"}-->
													<p class="raku2_deliv_status_1001"><a href="https://member.kms.kuronekoyamato.co.jp/parcel/detail?pno=<!--{$hasso_denpyo_code}-->" class="nav_btn deliv_track" target="_blank"><i class="fa fa-truck" aria-hidden="true"></i> 配送状況<!--{$hasso_denpyo_code_no}-->を確認</a></p>
												<!--{elseif $item.plg_raku2_hasso_deliv_id == "1002"}-->
													<p class="raku2_deliv_status_1002"><a href="http://k2k.sagawa-exp.co.jp/p/web/okurijosearch.do?okurijoNo=<!--{$hasso_denpyo_code}-->" class="nav_btn deliv_track" target="_blank"><i class="fa fa-truck" aria-hidden="true"	target="_blank"></i> 配送状況<!--{$hasso_denpyo_code_no}-->を確認</a></p>
												<!--{elseif $item.plg_raku2_hasso_deliv_id == "1003"}-->
													<p class="raku2_deliv_status_1003"><a href="http://tracking.post.japanpost.jp/service/singleSearch.do?org.apache.struts.taglib.html.TOKEN=&amp;searchKind=S002&amp;locale=ja&amp;SVID=&amp;reqCodeNo1=<!--{$hasso_denpyo_code}-->" class="nav_btn deliv_track"  target="_blank"><i class="fa fa-truck" aria-hidden="true"></i> 配送状況<!--{$hasso_denpyo_code_no}-->を確認</a></p>
												<!--{elseif $item.plg_raku2_hasso_deliv_id == "1004"}-->
													<p class="raku2_deliv_status_1004"><a href="https://track.seino.co.jp/kamotsu/KamotsuPrintServlet?ACTION=LIST&amp;GNPNO1=<!--{$hasso_denpyo_code|h}-->" class="nav_btn deliv_track" target="_blank"><i class="fa fa-truck" aria-hidden="true"></i> 配送状況<!--{$hasso_denpyo_code_no}-->を確認</a></p>
確認</a>									<!--{elseif $item.plg_raku2_hasso_deliv_id == "1006"}-->
													<p class="raku2_deliv_status_1006"><a href="https://corp.fukutsu.co.jp/situation/tracking_no_hunt/<!--{$hasso_denpyo_code|h}-->" class="nav_btn deliv_track" target="_blank"><i class="fa fa-truck" aria-hidden="true"></i> 配送状況<!--{$hasso_denpyo_code_no}-->を確認</a></p>
												<!--{elseif $item.plg_raku2_hasso_deliv_id == "1021"}-->
													<p class="raku2_deliv_status_1021"><a href="https://info.jpexpress.jp/confirm/confirmList.html?denpyoNo=<!--{$hasso_denpyo_code}-->" class="nav_btn deliv_track"  target="_blank"><i class="fa fa-truck" aria-hidden="true"></i> 配送状況<!--{$hasso_denpyo_code_no}-->を確認</a></p>
												<!--{else}-->
													<p class="raku2_deliv_status_other">伝票番号：<!--{$item.plg_raku2_hasso_denpyo_code}--></p>
												<!--{/if}-->
											<!--{/foreach}-->
										<!--{/if}-->
										
										<!--{* 問い合わせ *}-->
										<!--{if $raku2param.mypage_order_contact_flg==1}-->
											<a href="/contact/?order_id=<!--{$item.order_id}-->&mode=contact" class="nav_btn contact_btn"><i class="fa fa-envelope-o" aria-hidden="true"></i><span class="raku2_mypage_top_order_contact_btn"> 注文に関するお問い合わせ</span></a>
										<!--{/if}-->

										<!--{* キャンセルボタン *}-->
										<!--{if $license.raku_mypage_order_cancel==1}-->
											<!--{if $item.order_cancel}-->
												<a href="order_cancel.php?order_id=<!--{$item.order_id|h}-->&before=top" class="nav_btn"><i class="fa fa-caret-right" aria-hidden="true"></i><span class="raku2_mypage_top_order_cancel_btn"> ご注文をキャンセルする</span></a>
											<!--{/if}-->
										<!--{/if}-->

										<!--{* お客様への連絡事項 *}-->
										<!--{if $item.mail_message}-->
											<a href="javascript:void(0);" onclick="eccube.changeAction('<!--{$smarty.const.ROOT_URLPATH}-->mypage/history.php?order_id=<!--{$item.order_id}-->&mail_message=1', 'form1'); eccube.setValueAndSubmit('form1', 'order_id', '<!--{$item.order_id}-->'); return false;" class="nav_btn"><i class="fa fa-caret-right" aria-hidden="true"></i><span class="raku2_mypage_top_order_detail_btn"> 連絡事項があります</span></a>

										<!--{/if}-->
									</div>
								</div>
								<!--{if $license.raku_cart_item_addition==1}-->
									<!--▼購入フロー項目追加 -->
									<div class="raku2_cart_item_addition">
										<!--{include file="`$smarty.const.TEMPLATE_REALDIR`frontparts/item_addition_display.tpl" arrDisp=$item mode="history_list"}-->
									</div>
								<!--{/if}-->
							</div>
							<!--{/foreach}-->
							<p class="t_right"><i class="fa fa-angle-right" aria-hidden="true"></i> <a href="/mypage/history_list.php"><span class="plg_raku2_history_other_cnt">過去の購入履歴を見る</span></a></p>
						<!--{else}-->
							<p class="plg_raku2_not_history_cnt"><span class="plg_raku2_not_history_cnt_fh">　－ 現在、ご注文中の商品はありません。　</span><i class="fa fa-angle-right" aria-hidden="true"></i> <a href="/mypage/history_list.php"><span class="plg_raku2_not_history_cnt_lh">過去の購入履歴を見る</span></a></p>
						<!--{/if}-->
					</div>
				</div>
			</form>
			
			<!--{* 期間限定ポイントのライセンス有の場合に表示 *}-->
			<!--{if $raku2param.raku_entry_point=="1"}-->
				<!--{if $arrCampaign|@count_php8 > 0}-->
					<div id="campaign_info">
						<h3>開催中のポイントアップキャンペーン</h3>
						<table summary="キャンペーン" class="tbl_history">
							<colgroup width="20%"></colgroup>
							<colgroup width="60%"></colgroup>
							<colgroup width="20%"></colgroup>
							<tr>
								<th>キャンペーンコード</th>
								<th>キャンペーン内容</th>
								<th>エントリー</th>
							</tr>
							<!--{section name=cnt loop=$arrCampaign}-->
								<tr>
									<td class="alignC"><a href="<!--{$arrCampaign[cnt].url|h}-->"><!--{$arrCampaign[cnt].entpoint_code|h}--></a><br></td>
									<td>
										<span class="st">≪キャンペーン名≫</span><br />
										&nbsp;&nbsp;&nbsp;&nbsp;<!--{$arrCampaign[cnt].entpoint_name}--><br /><br />
										<span class="st">キャンペーン期間</span>：<br />
										&nbsp;&nbsp;&nbsp;&nbsp;<!--{$arrCampaign[cnt].start_date|sfDispDBDate}-->～<!--{$arrCampaign[cnt].end_date|sfDispDBDate}-->まで<br /><br />
										<span class="st">ポイントご利用期間</span>：<br />
										&nbsp;&nbsp;&nbsp;&nbsp;<!--{$arrCampaign[cnt].point_grant_date|sfDispDBDate}-->～<!--{$arrCampaign[cnt].point_disappear_date|sfDispDBDate}-->まで<br />
										<!--{if $arrCampaign[cnt].condition3==2}-->
											<br /><span class="st">キャンペーン対象商品カテゴリー</span>：<!--{$arrCampaign[cnt].condition3_category_name}-->
										<!--{elseif $arrCampaign[cnt].condition3==3}-->
											<br /><span class="st">キャンペーン対象商品</span>：<!--{$arrCampaign[cnt].condition3_category_name}-->
											<!--{foreach from=$arrCampaign[cnt].condition3_category_products item=item_detail}-->
												<!--{if $raku2param.no_image_opt=="1" && $item_detail.main_large_image==""}-->
												<!--{else}-->

													<!--{assign var=products_path value=$item_detail.main_large_image|sfNoImageMainList}-->
													<!--{if strpos($item_detail.main_large_image, $smarty.const.EXTERNAL_URL) === false}-->
														<!--{assign var=products_path value=$smarty.const.IMAGE_SAVE_URLPATH|cat:$products_path}-->
													<!--{/if}-->

													<!--{if $raku2param.products_page_url_type==1}-->
														<a href="/products/detail/<!--{$item_detail.product_id}-->.html" target="_blank" class="item_image">
															<img src="<!--{$products_path|h}-->" alt="<!--{if strlen($item_detail.plg_raku2_main_large_image_alt) > 0}--><!--{$item_detail.plg_raku2_main_large_image_alt|h}--><!--{else}--><!--{$item_detail.product_name|h}--><!--{/if}-->" class="js-replace-no-image-mini" />
															<!--{$item_detail.name}-->
														</a>
													<!--{elseif $raku2param.products_page_url_type==2}-->
														<a href="/products/<!--{$item_detail.plg_raku2_common_code|escape|urlencode}-->/" target="_blank" class="item_image">
															<img src="<!--{$products_path|h}-->" alt="<!--{if strlen($item_detail.plg_raku2_main_large_image_alt) > 0}--><!--{$item_detail.plg_raku2_main_large_image_alt|h}--><!--{else}--><!--{$item_detail.product_name|h}--><!--{/if}-->" class="js-replace-no-image-mini" />
															<!--{$item_detail.name}-->
														</a>
													<!--{else}-->
														<a href="/products/detail.php?product_id=<!--{$item_detail.product_id}-->" target="_blank" class="item_image">
															<img src="<!--{$products_path|h}-->" alt="<!--{if strlen($item_detail.plg_raku2_main_large_image_alt) > 0}--><!--{$item_detail.plg_raku2_main_large_image_alt|h}--><!--{else}--><!--{$item_detail.product_name|h}--><!--{/if}-->" class="js-replace-no-image-mini" />
															<!--{$item_detail.name}-->
														</a>
													<!--{/if}-->
												<!--{/if}-->

											<!--{/foreach}-->
										<!--{elseif $arrCampaign[cnt].condition3==4}-->
											<br /><span class="st">キャンペーン対象外の商品カテゴリー</span>：<!--{$arrCampaign[cnt].condition3_category_name}-->
										<!--{elseif $arrCampaign[cnt].condition3==5}-->
											<br /><span class="st">キャンペーン対象外の対象商品</span>：<!--{$arrCampaign[cnt].condition3_category_name}-->
											<!--{foreach from=$arrCampaign[cnt].condition3_category_products item=item_detail}-->
												<!--{* 対象外商品の場合はリンクはなし *}-->
												<!--{$item_detail.name}-->
											<!--{/foreach}-->
										<!--{/if}-->
									</td>
									<td class="alignC">
										<!--{if $arrCampaign[cnt].issuance_type=="1"}-->
											<!--{if $arrCampaign[cnt].mycampaign=="1"}-->
												エントリー済<br /><!--{if $arrCampaign[cnt].order_count==0}-->（未購入）<!--{/if}-->
											<!--{else}-->
												<a href="/campaign/entry.php?campaign=<!--{$arrCampaign[cnt].entpoint_code}-->">エントリーする</a>
											<!--{/if}-->
										<!--{else}-->
												エントリー不要<br /><!--{if $arrCampaign[cnt].order_count==0}-->（未購入）<!--{/if}-->
										<!--{/if}-->
									</td>
								</tr>
							<!--{/section}-->
						</table>
					</div>
				<!--{/if}-->
			<!--{/if}-->
			
		<!--{/if}-->
		
		<!--{if $tpl_customer_type==constant("Raku2::CUSTOMER_TYPE_MEMBER")}-->
			<!--{$raku2param.message_pc_mypage_member_top_lower}-->
		<!--{elseif $tpl_customer_type==constant("Raku2::CUSTOMER_TYPE_MEMBER_GROUP")}-->
			<!--{$raku2param.message_pc_mypage_group_top_lower}-->
		<!--{else}-->
			<!--{$raku2param.message_pc_mypage_top_lower}-->
		<!--{/if}-->
		
	</div>
</div>
