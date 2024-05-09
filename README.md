# Simple-To-Do
//importlar

import Nat "mo:base/Nat";
import Nat64 "mo:base/Nat64";
import Cycles "no:base/ExperimentalCycles";

//canister
//gas fee (cycle)

actor HelloCycles {
  //değişkenler
  //let -> immutable (değiştiremiyorsun)
  //var -> mutable
  let limit = 10_000_000;

  //fonksiyonlar(query,update)
  public func wallet_balance() : async Nat {
    //return -> return ...; veya ...
    Cycles.balance() //return Cycles.balance();
  };
  public func wallet_recieve() : async { accepted : Nat64 } {
    let available = Cycles.available();
    let accepted = Cycles.accept(Nat.min(available, limit));
    { accepted = Nat64.fromNat(accepted) };
  };
  public func transfer(
    reciever : shared () -> async (),
    amount : Nat,
  ) : async { refunded : Nat } {
    Cycles.add(amount);
    await receiever();
    { refunded = Cycles.refunded() };
  };
};
