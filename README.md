<div class="raku2_theme01">
    <div id="header_wrap" >
        <p id="site_description">キュテラ株式会社</p>

        <div id="header">

            <div class="logo_area -flex3">
                <h1><a href="<!--{$smarty.const.TOP_URLPATH}-->"><img src="<!--{$smarty.const.IMAGE_SAVE_URLPATH}--><!--{$shop_logo_image_pc}-->" alt="<!--{$tpl_h1|h}-->" /></a></h1>
            </div><!-- .logo_area[end] -->

            <!--{* ▼HeaderInternal COLUMN *}-->
            <!--{if $arrPageLayout.HeaderInternalNavi|@count_php8 > 0}-->
                <!--{* ▼上ナビ *}-->
                <!--{foreach key=HeaderInternalNaviKey item=HeaderInternalNaviItem from=$arrPageLayout.HeaderInternalNavi}-->
                    <!-- ▼<!--{$HeaderInternalNaviItem.bloc_name}--> -->
                    <!--{if $HeaderInternalNaviItem.php_path != ""}-->
                        <!--{include_php file=$HeaderInternalNaviItem.php_path items=$HeaderInternalNaviItem}-->
                    <!--{else}-->
                        <!--{include file=$HeaderInternalNaviItem.tpl_path items=$HeaderInternalNaviItem}-->
                    <!--{/if}-->
                    <!-- ▲<!--{$HeaderInternalNaviItem.bloc_name}--> -->
                <!--{/foreach}-->
                <!--{* ▲上ナビ *}-->
            <!--{/if}-->
            <!--{* ▲HeaderInternal COLUMN *}-->
            
        <!--{if $tpl_login}-->

            <div class="block_outer -flex0">
                <div id="header_login_area">
                    <form name="header_login_form" id="header_login_form" method="post" action="<!--{$smarty.const.HTTPS_URL}-->frontparts/login_check.php"<!--{if !$tpl_login}--> onsubmit="return eccube.checkLoginFormInputted("header_login_form")"<!--{/if}-->>
                        <input type="hidden" name="mode" value="login" />
                        <input type="hidden" name="<!--{$smarty.const.TRANSACTION_ID_NAME}-->" value="<!--{$transactionid}-->" />
                        <input type="hidden" name="url" value="<!--{$smarty.server.REQUEST_URI|h}-->" />
                        <div id="header_userarea">
                            <div class="userarea_name">
                                <p><!--{$tpl_company_name}--> <!--{$tpl_company_department}--></p>
                                <p> <!--{$tpl_name1}--><!--{$tpl_name2}--> 様</p>
                            </div>
                            <!--{if !$tpl_disable_logout}-->
                            <div class="userarea_cart_btn">
                                <p class="quantity"><!--{$tpl_arrCartList.TotalQuantity|number_format_ex|default:0}--></p>
                                <a href="<!--{$smarty.const.ROOT_URLPATH}-->cart/"><img src="<!--{$TPL_URLPATH}-->img/common/icon-img01.png" alt="カート">カート</a>
                            </div>
                            <div class="userarea_mypage_btn">
                                <a href="<!--{$smarty.const.HTTPS_URL}-->mypage/"><img src="<!--{$TPL_URLPATH}-->img/common/icon-img04.png" alt="マイページ">MYページ</a>
                            </div>
                            <!--{/if}-->
                        </div>
                    </form>
                </div>
            </div><!-- .block_outer[end] -->
        </div><!-- #header[end] -->

        <!--{else}-->

            <div class="block_outer -flex0">
                <div id="header_login_area">
                    <form name="header_login_form" id="header_login_form" method="post" action="<!--{$smarty.const.HTTPS_URL}-->frontparts/login_check.php"<!--{if !$tpl_login}--> onsubmit="return eccube.checkLoginFormInputted("header_login_form")"<!--{/if}-->>
                        <input type="hidden" name="mode" value="login" />
                        <input type="hidden" name="<!--{$smarty.const.TRANSACTION_ID_NAME}-->" value="<!--{$transactionid}-->" />
                        <input type="hidden" name="url" value="<!--{$smarty.server.REQUEST_URI|h}-->" />
                        <div id="header_userarea">
                            <div class="userarea_cart_btn">
                                <p class="quantity"><!--{$tpl_arrCartList.TotalQuantity|number_format_ex|default:0}--></p>
                                <a href="<!--{$smarty.const.ROOT_URLPATH}-->cart/"><img src="<!--{$TPL_URLPATH}-->img/common/icon-img01.png" alt="カート">カート</a>
                            </div>
                            <div class="userarea_new_btn">
                                <a href="<!--{$smarty.const.ROOT_URLPATH}-->entry/"><img src="<!--{$TPL_URLPATH}-->img/common/icon-img02.png" alt="新規お取引">新規お取引</a>
                            </div>
                            <div class="userarea_login_btn">
                                <a href="<!--{$smarty.const.HTTPS_URL}-->mypage/member_login.php"><img src="<!--{$TPL_URLPATH}-->img/common/icon-img03.png" alt="ログイン">ログイン</a>
                            </div>
                        </div>
                    </form>
                </div>
            </div><!-- .block_outer[end] -->
        </div><!-- #header[end] -->

        <div id="header_benefits">
            <a href="<!--{$smarty.const.ROOT_URLPATH}-->entry/">
             
                </ul>
            </a>
        </div><!-- #header_benefits[end] -->  

        <!--{/if}-->
        <div id="gloval_navi">
            <ul class="shoppingt_btn">
                <!--<li class="header_cat_btn"><a href="javascript:void(0)">商品カテゴリ</a><!--{include file="`$smarty.const.TEMPLATE_REALDIR`frontparts/plg_Raku2_global_products.tpl row=2 column=3 level=3 trees=$arrGlobalTree}--></li>-->
                <li><a href="<!--{$smarty.const.ROOT_URLPATH}-->mypage/upload_order.php">クイックオーダー</a></li>
                <li><a href="<!--{$smarty.const.ROOT_URLPATH}-->mypage/favorite.php">お気に入り</a></li>
                <li><a href="<!--{$smarty.const.ROOT_URLPATH}-->mypage/history_list.php">発注履歴</a></li>
            </ul>
            <ul class="use_btn">
                <li><a href="<!--{$smarty.const.ROOT_URLPATH}-->introduction.html">初めての方へ</a></li>
                <li><a href="<!--{$smarty.const.ROOT_URLPATH}-->user_data/guide.php">ご利用ガイド</a></li>
                <li><a href="<!--{$smarty.const.ROOT_URLPATH}-->faq.html">よくある質問</a></li>
                <li><a href="<!--{$smarty.const.ROOT_URLPATH}-->contact/?mode=contact">お問い合わせ</a></li>
            </ul>
        </div><!-- #gloval_navi[end] -->
    </div><!-- #header_wrap[end] -->

    <!--{if $smarty.server.PHP_SELF == "/index.php"}-->
        <!--{if $raku2param.slider_library_for_pc == '1'}-->
            <div class="slider_area">
                <ul class="bnr-slider">
                    <!-- 各スライド -->
                    <li><img src="<!--{$TPL_URLPATH}-->img/common/slider01.jpg" /></li>
                    <li><img src="<!--{$TPL_URLPATH}-->img/common/slider02.jpg" /></li>
                    <li><img src="<!--{$TPL_URLPATH}-->img/common/slider03.jpg" /></li>
                </ul>
            </div>
            <!--{else}-->
            <div id="main_slide_image" class="design-style">
                <ul class="bxslider">
                    <!-- 各スライド -->
                    <li><img src="<!--{$TPL_URLPATH}-->img/common/slider01.jpg" /></li>
                    <li><img src="<!--{$TPL_URLPATH}-->img/common/slider02.jpg" /></li>
                    <li><img src="<!--{$TPL_URLPATH}-->img/common/slider03.jpg" /></li>
                </ul>
            </div>
        <!--{/if}-->
    <!--{/if}-->

</div><!-- .raku2_theme01[end] -->
